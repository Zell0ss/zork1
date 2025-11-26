# Object Location Constraints in Zork I Parser

This document explains the **location constraints** used in Zork I's parser syntax definitions. These are not physical locations, but rather **search filters** that control where the parser looks for objects when resolving player commands.

---

## Overview

When you define a verb's syntax, you specify **where objects can be** for that verb to work on them. These constraints ensure verbs behave realistically:

- **DROP** only works on items you're holding
- **ATTACK** only works on creatures in your current room
- **EXAMINE** works on anything you can see
- **EAT** automatically takes food if you're not holding it

---

## The Location Constraint System

### Bit Flags (Internal Implementation)

From [gparser.zil:1032-1038](gparser.zil#L1032-L1038):

```python
# Bit flags for object location constraints
SH = 128       # Search here (internal use)
SC = 64        # Search containers (internal use)
SIR = 32       # Search IN-ROOM
SOG = 16       # Search ON-GROUND
STAKE = 8      # Search TAKE (auto-take if needed)
SMANY = 4      # Allow MANY objects
SHAVE = 2      # Search HAVE (must already be held)
```

### How They're Used in Syntax

In `SYNTAX` declarations, location constraints are specified as lists:

```python
syntax_drop = SyntaxRule(
    verb="DROP",
    direct_object=ObjectSpec(
        location=["HELD", "MANY", "HAVE"]  # Location constraints
    ),
    action=v_drop
)
```

---

## Location Constraint Types

### 1. HELD / HAVE / CARRIED (SHAVE bit = 2)


**Meaning**: Object must **already be in player's inventory** object.location == WINNER

**Used for**: Verbs that only work on items you're carrying

**Parser behavior**: **Rejects** the command if object not in inventory


They're the same - just different words for the same concept, like how English has "start/begin" or "big/large". The ZIL parser treats them identically, but developers chose the word that sounded most natural for each verb. This is actually good API design - it makes the syntax definitions read more naturally, even though under the hood it's all just one bit flag (SHAVE = 2) checking if object.location == WINNER


#### Examples

```python
# DROP - can only drop what you're holding
syntax_drop = SyntaxRule(
    verb="DROP",
    direct_object=ObjectSpec(location=["HELD", "MANY", "HAVE"]),
    action=v_drop
)

# WEAR - can only wear what you're carrying
syntax_wear = SyntaxRule(
    verb="WEAR",
    direct_object=ObjectSpec(flags=[WEARBIT], location=["HELD"]),
    action=v_wear
)

# THROW - must be holding item to throw it
syntax_throw = SyntaxRule(
    verb="THROW",
    direct_object=ObjectSpec(location=["HELD", "HAVE"]),
    action=v_throw
)

# ATTACK WITH - weapon must be carried
syntax_attack = SyntaxRule(
    verb="ATTACK",
    direct_object=ObjectSpec(flags=[ACTORBIT], location=["ON_GROUND", "IN_ROOM"]),
    indirect_object=ObjectSpec(flags=[WEAPONBIT], location=["HELD", "CARRIED", "HAVE"]),
    action=v_attack
)

# DRINK FROM - container must be carried
syntax_drink_from = SyntaxRule(
    verb="DRINK",
    preposition="FROM",
    direct_object=ObjectSpec(location=["HELD", "CARRIED"]),
    action=v_drink_from
)
```

#### Player Experience

```
> DROP LAMP
(lamp is in inventory - works)
Dropped.

> DROP SKULL
(skull is in room but not held - fails)
You don't have the crystal skull.

> WEAR ROBE
(robe in room - fails)
You don't have the robe.

> TAKE ROBE
Taken.

> WEAR ROBE
(now held - works)
You are now wearing the robe.


> ATTACK TROLL WITH SWORD
(sword in inventory - works)
You swing the sword at the troll...

> ATTACK TROLL WITH AXE
(axe in room but not held - fails)
You don't have the axe.
```

---


### 2. ON-GROUND (SOG bit = 16)

**Meaning**: Object is **on the floor in the current room**

**Used for**: Objects that must be in your current location (not held, not in containers)

**What it searches**: Objects whose direct location is the current room

#### Examples

```python
# ATTACK - can only attack NPCs in the same room
syntax_attack = SyntaxRule(
    verb="ATTACK",
    direct_object=ObjectSpec(flags=[ACTORBIT], location=["ON_GROUND", "IN_ROOM"]),
    action=v_attack
)

# GIVE TO - recipient must be in room
syntax_give = SyntaxRule(
    verb="GIVE",
    direct_object=ObjectSpec(location=["MANY", "HELD", "HAVE"]),
    preposition="TO",
    indirect_object=ObjectSpec(flags=[ACTORBIT], location=["ON_GROUND"]),
    action=v_give
)

# BOARD - vehicle must be in room
syntax_board = SyntaxRule(
    verb="BOARD",
    direct_object=ObjectSpec(flags=[VEHBIT], location=["ON_GROUND", "IN_ROOM"]),
    action=v_board
)
```

#### Player Experience

```
> ATTACK TROLL
(troll is in room - works)
You swing at the troll...

> ATTACK GRUE
(grue is global but not physically present)
I don't see any grue here!

> GIVE SWORD TO THIEF
(thief not in room - fails)
I don't see any thief here!

> GIVE SWORD TO THIEF
(thief appears in room - works)
The thief accepts your sword reluctantly.
```

---

### 3. IN-ROOM (SIR bit = 32)

**Meaning**: Object is **visible/accessible** in current room

**Used for**: More permissive searching than ON-GROUND

**What it searches**:
- Objects directly in room
- Objects in player's inventory
- Global objects (GLOBAL-OBJECTS)
- Local-global objects (if room declares them)
- Objects in **open** containers in room

#### Examples

```python
# EXAMINE - can examine anything visible
syntax_examine = SyntaxRule(
    verb="EXAMINE",
    direct_object=ObjectSpec(location=["IN_ROOM", "HELD", "CARRIED", "MANY"]),
    action=v_examine
)

# OPEN - can open doors, containers in room or held
syntax_open = SyntaxRule(
    verb="OPEN",
    direct_object=ObjectSpec(flags=[DOORBIT], location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM"]),
    action=v_open
)

# TAKE - can take objects in room
syntax_take = SyntaxRule(
    verb="TAKE",
    direct_object=ObjectSpec(flags=[TAKEBIT], location=["ON_GROUND", "IN_ROOM"], many=True),
    action=v_take
)
```

#### What "IN-ROOM" Finds

From [gparser.zil:1372-1396](gparser.zil#L1372-L1396), the `ACCESSIBLE?` routine:

```python
def accessible(obj):
    """Check if player can access/touch an object (IN-ROOM test)"""

    # 1. Invisible objects are never accessible
    if obj.has_flag(INVISIBLE):
        return False

    location = obj.location

    # 2. No location = not accessible
    if not location:
        return False

    # 3. Global objects are ALWAYS accessible (anywhere)
    if location == GLOBAL_OBJECTS:
        return True

    # 4. Local-globals accessible if room declares them
    if location == LOCAL_GLOBALS:
        if obj in HERE.global_objects:
            return True
        return False

    # 5. Object must be in current room or player's inventory tree
    meta_location = find_topmost_container(obj)
    if meta_location not in [HERE, WINNER.location]:
        return False

    # 6. Check direct locations
    if location in [WINNER, HERE, WINNER.location]:
        return True

    # 7. Inside container - container must be OPEN and accessible
    if location.has_flag(OPENBIT) and accessible(location):
        return True

    return False
```

#### Player Experience

```
> EXAMINE LAMP
(lamp in inventory - works)
A battery-powered brass lantern.

> EXAMINE GROUND
(GROUND is global - works)
Just ordinary ground.

> EXAMINE PAINTING
(painting on wall in room - works)
A beautiful forest scene.

> EXAMINE WATER
(water inside closed bottle in room - fails if bottle closed)
You can't see inside the bottle.

> OPEN BOTTLE
The bottle is now open.

> EXAMINE WATER
(now visible through open bottle - works)
It looks like ordinary water.
```

---

### 4. TAKE (STAKE bit = 8)

**Meaning**: If object is **not held**, **automatically take it first**

**Used for**: Verbs that require holding object but should be helpful and auto-take

**Parser behavior**: Attempts to take object automatically, prints "(Taken)" if successful

#### Examples

```python
# EAT - auto-takes food if not held
syntax_eat = SyntaxRule(
    verb="EAT",
    direct_object=ObjectSpec(
        flags=[FOODBIT],
        location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM", "TAKE"]
    ),
    action=v_eat
)

# READ - auto-takes readable if not held
syntax_read = SyntaxRule(
    verb="READ",
    direct_object=ObjectSpec(
        flags=[READBIT],
        location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM", "TAKE"]
    ),
    action=v_read
)

# LIGHT - auto-takes light source
syntax_light = SyntaxRule(
    verb="LIGHT",
    direct_object=ObjectSpec(
        flags=[LIGHTBIT],
        location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM", "TAKE"]
    ),
    action=v_light
)
```

#### Auto-Take Logic

From [gparser.zil:1265-1291](gparser.zil#L1265-L1291):

```python
def itake_check(obj, location_bits):
    """Check if object needs to be taken first"""

    if not held(obj) and obj not in [HANDS, ME]:
        # Object not held

        if obj.has_flag(TRYTAKEBIT):
            # Looks takeable but isn't (scenery, etc.) - FAIL
            taken = True  # Pretend taken (will fail in verb handler)

        elif location_bits & STAKE:  # TAKE bit set in syntax
            # Auto-take the object
            if itake():  # Attempt to take it
                print("(Taken)")
                taken = False  # Success, continue
            else:
                taken = False  # Failed to take, will fail

        else:
            # TAKE bit not set, but not held
            print(f"You don't have the {obj.desc}.")
            return False

    return True
```

#### Player Experience

```
> EAT LUNCH
(lunch in room, auto-takes it)
(Taken)
Thank you very much. It really hit the spot.

> READ BOOK
(book already held - no auto-take message)
The book describes an ancient ritual...

> READ LEAFLET
(leaflet in room, auto-takes)
(Taken)
WELCOME TO ZORK!

> EAT GROUND
(GROUND has TRYTAKEBIT - looks takeable but isn't)
You can't take the ground!
(EAT fails because couldn't take it)
```

#### Objects with TRYTAKEBIT

Some objects **look** takeable but aren't (to filter TAKE ALL spam):

```python
class GROUND(Object):
    location = GLOBAL_OBJECTS
    synonyms = ["GROUND", "DIRT", "SAND"]
    description = "ground"
    flags = [TRYTAKEBIT]  # Prevents "You can't take the ground!" on every TAKE ALL

# Player tries:
# > TAKE ALL
# lamp: Taken.
# sword: Taken.
# (ground silently skipped because TRYTAKEBIT)
```

---

### 6. MANY (SMANY bit = 4)

**Meaning**: Allow **multiple objects** (ALL, multiple nouns)

**Used for**: Verbs that can handle multiple objects at once

**Parser behavior**: Loops through all matching objects

#### Examples

```python
# TAKE ALL
syntax_take = SyntaxRule(
    verb="TAKE",
    direct_object=ObjectSpec(
        flags=[TAKEBIT],
        location=["ON_GROUND", "IN_ROOM"],
        many=True  # MANY flag set
    ),
    action=v_take
)

# DROP ALL
syntax_drop = SyntaxRule(
    verb="DROP",
    direct_object=ObjectSpec(
        location=["HELD", "MANY", "HAVE"]
    ),
    action=v_drop
)

# EXAMINE multiple objects
syntax_examine = SyntaxRule(
    verb="EXAMINE",
    direct_object=ObjectSpec(
        location=["IN_ROOM", "HELD", "CARRIED", "MANY"]
    ),
    action=v_examine
)
```

#### Player Experience

```
> TAKE ALL
brass lantern: Taken.
elvish sword: Taken.
rope: Taken.
brown sack: Taken.

> DROP ALL
brass lantern: Dropped.
elvish sword: Dropped.
rope: Dropped.
brown sack: Dropped.

> EXAMINE LAMP AND SWORD
brass lantern: A battery-powered brass lantern.
elvish sword: An elvish sword of great antiquity.

> PUT ALL IN SACK
brass lantern: Done.
rope: Done.
```

#### MANY Without Multi-Object Support

```python
# ATTACK doesn't support MANY
syntax_attack = SyntaxRule(
    verb="ATTACK",
    direct_object=ObjectSpec(
        flags=[ACTORBIT],
        location=["ON_GROUND", "IN_ROOM"]
        # No many=True!
    ),
    action=v_attack
)

# Player tries:
# > ATTACK TROLL AND THIEF
# You can't use multiple direct objects with "attack".
```

---

## Combining Location Constraints

Most verbs combine multiple constraints for maximum flexibility:

### Example 1: UNLOCK Command

```python
# UNLOCK <door> WITH <key>
syntax_unlock = SyntaxRule(
    verb="UNLOCK",
    direct_object=ObjectSpec(
        location=["ON_GROUND", "IN_ROOM"]  # Door can be in room
    ),
    preposition="WITH",
    indirect_object=ObjectSpec(
        flags=[TOOLBIT],
        location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM", "TAKE"]  # Key anywhere, will auto-take
    ),
    action=v_unlock
)
```

**Player experience:**
```
> UNLOCK GRATE WITH KEY
(key in room, auto-takes it)
(Taken)
The grate is unlocked.

> UNLOCK DOOR WITH KEY
(key already held, no message)
The door is unlocked.
```

### Example 2: BURN WITH Command

```python
# BURN <object> WITH <fire source>
syntax_burn = SyntaxRule(
    verb="BURN",
    direct_object=ObjectSpec(
        flags=[BURNBIT],
        location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM"]  # Thing to burn
    ),
    preposition="WITH",
    indirect_object=ObjectSpec(
        flags=[FLAMEBIT],
        location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM", "HAVE"]  # Fire source
    ),
    action=v_burn
)
```

**Player experience:**
```
> BURN BOOK WITH MATCH
(both objects accessible)
The book begins to burn...

> BURN LEAVES WITH TORCH
(torch in room, leaves in room)
The leaves catch fire briefly.
```

### Example 3: PUT IN Command

```python
# PUT <object> IN <container>
syntax_put = SyntaxRule(
    verb="PUT",
    direct_object=ObjectSpec(
        location=["HELD", "MANY", "HAVE"]  # Must hold object to put it
    ),
    preposition="IN",
    indirect_object=ObjectSpec(
        flags=[CONTBIT],
        location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM"]  # Container anywhere accessible
    ),
    action=v_put
)
```

**Player experience:**
```
> PUT SWORD IN SACK
(sword held, sack held)
Done.

> PUT LAMP IN CASE
(lamp held, trophy case in room)
Done.

> PUT JEWELS IN COFFIN
(jewels in room, not held - fails)
You don't have the jewels.
```

---

## Location Constraint Hierarchy

The constraints form a **hierarchy of accessibility** from most to least restrictive:

### Restrictiveness Levels

```
Most Restrictive:
    ↓
1. HELD / HAVE
   └─ Must already be in player's inventory
   └─ Example: DROP, WEAR

2. CARRIED
   └─ Similar to HELD (carrying implies holding in Zork I)
   └─ Example: ATTACK WITH weapon

3. ON-GROUND
   └─ Must be on floor in current room only
   └─ Excludes: inventory, containers, other rooms
   └─ Example: GIVE TO actor, BOARD vehicle

4. IN-ROOM
   └─ Anything visible in current context
   └─ Includes: room contents, inventory, globals, open containers
   └─ Example: EXAMINE, TAKE

5. TAKE (with auto-take)
   └─ Like IN-ROOM but helpfully grabs object first
   └─ Example: EAT, READ, LIGHT
    ↓
Least Restrictive
```

---

## Summary Table

| Constraint | Bit | Where Parser Searches | Auto-Take? | Typical Use Cases |
|------------|-----|----------------------|------------|-------------------|
| **HELD** / **HAVE** / **CARRIED**| 2 (SHAVE) | Player's inventory only | No | DROP, WEAR, THROW items you're holding, ATTACK WITH weapon, DRINK FROM container |
| **ON-GROUND** | 16 (SOG) | Current room floor only | No | ATTACK creatures, GIVE TO actors, BOARD vehicles |
| **IN-ROOM** | 32 (SIR) | Room + inventory + globals + open containers | No | EXAMINE, TAKE, OPEN anything visible |
| **TAKE** | 8 (STAKE) | Anywhere accessible | **Yes** | EAT, READ, LIGHT (auto-takes if needed) |
| **MANY** | 4 (SMANY) | Allows multiple objects | Depends | TAKE ALL, DROP ALL, PUT ALL IN |

---

## Design Benefits

This location constraint system provides several advantages:

### 1. **Realistic Behavior**

Verbs behave as expected:
- Can't drop what you're not holding
- Can't attack creatures in other rooms
- Can examine anything you can see

### 2. **Player Convenience**

TAKE constraint auto-grabs objects:
```
> READ BOOK
(Taken)
The book says...
```

Instead of forcing tedious:
```
> READ BOOK
You're not holding the book.
> TAKE BOOK
Taken.
> READ BOOK
The book says...
```

### 3. **Reduced Error Messages**

TRYTAKEBIT filters TAKE ALL:
```
> TAKE ALL
lamp: Taken.
sword: Taken.
(ground, walls, trees silently skipped)
```

Instead of spamming:
```
> TAKE ALL
lamp: Taken.
sword: Taken.
ground: You can't take that!
wall: You can't take that!
tree: You can't take that!
```

### 4. **Precise Verb Control**

Designers can precisely control each verb's behavior:
- **DROP** requires HELD (prevent dropping what you don't have)
- **ATTACK** requires ON-GROUND (can't attack distant enemies)
- **EXAMINE** allows IN-ROOM (examine anything visible)
- **EAT** includes TAKE (helpful auto-take)

### 5. **Flexible Combinations**

Constraints can be combined for complex scenarios:
```python
# UNLOCK requires:
# - Door in room (ON-GROUND, IN-ROOM)
# - Key anywhere, will auto-take (TAKE)
indirect_object=ObjectSpec(
    flags=[TOOLBIT],
    location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM", "TAKE"]
)
```

---

## Technical Implementation Notes

### How Constraints Are Checked

The parser checks constraints in this order:

1. **Parse command** → Extract verb and objects
2. **Syntax matching** → Find matching SYNTAX rule
3. **Object resolution** → Find objects matching words
4. **Location filtering** → Apply location constraints
5. **Auto-take check** → If TAKE bit set, attempt auto-take
6. **MANY expansion** → If MANY bit set, expand to multiple objects
7. **Action dispatch** → Call verb's action routine

### ACCESSIBLE? Routine

From [gparser.zil:1372-1396](gparser.zil#L1372-L1396):

```python
def accessible(obj):
    """
    Core routine that determines if object passes IN-ROOM constraint

    Returns True if:
    - Object in GLOBAL-OBJECTS (always accessible)
    - Object in LOCAL-GLOBALS and room declares it
    - Object in current room
    - Object in player's inventory
    - Object in open container in room or inventory

    Returns False if:
    - Object is INVISIBLE
    - Object in different room
    - Object in closed container
    """
    if obj.has_flag(INVISIBLE):
        return False

    location = obj.location

    if not location:
        return False

    # Global objects always accessible
    if location == GLOBAL_OBJECTS:
        return True

    # Local-globals if room declares them
    if location == LOCAL_GLOBALS:
        return obj in HERE.global_objects

    # Must be in current room or inventory tree
    meta_location = find_topmost_container(obj)
    if meta_location not in [HERE, WINNER.location]:
        return False

    # Direct locations
    if location in [WINNER, HERE, WINNER.location]:
        return True

    # In container - must be open
    if location.has_flag(OPENBIT) and accessible(location):
        return True

    return False
```

---

## Common Patterns

### Pattern 1: "Anywhere Accessible with Auto-Take"

Most permissive - used for convenience verbs:

```python
location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM", "TAKE"]
```

Examples: EAT, READ, LIGHT, RING

### Pattern 2: "Must Be Held"

Most restrictive - used for possession verbs:

```python
location=["HELD", "HAVE"]
```

Examples: DROP, THROW, WEAR

### Pattern 3: "In Room or Held"

Common middle ground:

```python
location=["HELD", "CARRIED", "ON_GROUND", "IN_ROOM"]
```

Examples: OPEN, CLOSE, EXAMINE, TAKE

### Pattern 4: "Must Be in Room"

For verbs that target room objects only:

```python
location=["ON_GROUND", "IN_ROOM"]
```

Examples: ATTACK (target), GIVE TO (recipient), BOARD

### Pattern 5: "With MANY Support"

For batch operations:

```python
location=["ON_GROUND", "IN_ROOM"],
many=True
```

Examples: TAKE ALL, DROP ALL, EXAMINE multiple

---

## Conclusion

The location constraint system is a key part of what makes Zork I's parser feel intelligent and helpful. By carefully choosing constraints for each verb, the game:

- **Enforces realistic behavior** (can't drop what you don't have)
- **Provides helpful shortcuts** (auto-takes objects when sensible)
- **Reduces frustration** (filters TAKE ALL spam)
- **Feels natural** (commands work as players expect)

This design pattern influenced decades of interactive fiction parsers and remains a elegant solution to the "where should objects be" problem in text adventure games.
