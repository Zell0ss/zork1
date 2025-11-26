# Global and Local-Global Object Systems in Zork I

This document explains how Zork uses special object containers to make certain objects accessible from multiple locations.

## What Are Global Objects?

**GLOBAL-OBJECTS** and **LOCAL-GLOBALS** are special container objects that make their children visible in specific contexts:

1. **GLOBAL-OBJECTS** - Objects visible **everywhere** in the game
2. **LOCAL-GLOBALS** - Objects visible only in **specific rooms** that declare them

## Why Are They Needed?

Without global objects, you'd need to:
- Duplicate scenery objects in every room (walls, ground, sky)
- Track and move NPCs/items that should be "everywhere"
- Write complex visibility logic for each room

Global objects solve this elegantly:
- Define once, visible in many places
- Centralized logic and action handlers
- Saves memory and reduces code duplication

---

## GLOBAL-OBJECTS (Always Visible)

Objects in GLOBAL-OBJECTS are accessible from **any room** in the game.

### Definition:
```python
class GLOBAL_OBJECTS(Object):
    """Container for objects visible everywhere"""
    flags = [RMUNGBIT, INVISIBLE, TOUCHBIT, SURFACEBIT,
             TRYTAKEBIT, OPENBIT, SEARCHBIT, TRANSBIT,
             ONBIT, RLANDBIT, FIGHTBIT, STAGGERED, WEARBIT]
```

### Complete List of Global Objects:

From [gglobals.zil](gglobals.zil):

```python
# Abstract/Conceptual Objects
INTNUM          # Numbers (for commands like COUNT)
IT              # Pronoun reference (IT, THEM, HER, HIM)
BLESSINGS       # Abstract concept for COUNT verb
ZORKMID         # Currency reference

# Universal Scenery
TEETH           # "Set of teeth" (scenery)
WALL            # Walls (in most rooms)
GRANITE_WALL    # Granite walls (specific rooms)
GROUND          # Ground/floor/dirt/sand
PATHOBJ         # Paths and trails

# Creatures/NPCs
GRUE            # The lurking grue (darkness)
SAILOR          # Sailor (for HELLO command)

# Player-Related
ME              # Self-reference (MYSELF, ME)
LUNGS           # For BLOW command
HANDS           # Your hands (TOOLBIT for bare-hand actions)
ADVENTURER      # Player template
```

### Example 1: The GRUE

From [gglobals.zil:184-206](gglobals.zil#L184-L206):

```python
class GRUE(Object):
    """The infamous lurking grue - visible everywhere"""
    location = GLOBAL_OBJECTS
    synonyms = ["GRUE"]
    adjectives = ["LURKING", "SINISTER", "HUNGRY", "SILENT"]
    description = "lurking grue"
    action = grue_function

def grue_function():
    """Handle grue interactions"""
    if current_verb == "EXAMINE":
        print("""The grue is a sinister, lurking presence in the dark places of the
earth. Its favorite diet is adventurers, but its insatiable
appetite is tempered by its fear of light. No grue has ever been
seen by the light of day, and few have survived its fearsome jaws
to tell the tale.""")

    elif current_verb == "FIND":
        print("""There is no grue here, but I'm sure there is at least one lurking
in the darkness nearby. I wouldn't let my light go out if I were
you!""")

    elif current_verb == "LISTEN":
        print("It makes no sound but is always lurking in the darkness nearby.")
```

**Why it's global:** Players can reference the grue from any dark location. It's a constant threat mentioned in the game's famous opening.

**Player experience:**
```
> EXAMINE GRUE
The grue is a sinister, lurking presence...

> (later, in a dark room with no light)
It is pitch black. You are likely to be eaten by a grue.
```

### Example 2: GROUND

From [gglobals.zil:164-182](gglobals.zil#L164-L182):

```python
class GROUND(Object):
    """Ground/floor - accessible everywhere"""
    location = GLOBAL_OBJECTS
    synonyms = ["GROUND", "SAND", "DIRT", "FLOOR"]
    description = "ground"
    action = ground_function

def ground_function():
    """Handle ground interactions"""
    if current_verb in ["PUT", "PUT_ON"] and PRSI == GROUND:
        # "PUT SWORD ON GROUND" -> DROP SWORD
        perform(V_DROP, PRSO)
        return True

    elif current_verb == "DIG":
        print("The ground is too hard for digging here.")
```

**Why it's global:** Every room has ground. Players might try to interact with it anywhere.

**Player experience:**
```
> PUT SWORD ON GROUND
Dropped.

> DIG IN GROUND
The ground is too hard for digging here.
```

### Example 3: IT (Pronoun Reference)

From [gglobals.zil:42-46](gglobals.zil#L42-L46):

```python
class IT(Object):
    """Pronoun for last referenced object"""
    location = GLOBAL_OBJECTS
    synonyms = ["IT", "THEM", "HER", "HIM"]
    description = "random object"
    flags = [NDESCBIT, TOUCHBIT]

# How it works:
# > EXAMINE SKULL
P_IT_OBJECT = SKULL  # IT now points to skull

# > TAKE IT
PRSO = IT  # Parser resolves IT to SKULL
# Executes: TAKE SKULL
```

**Why it's global:** Pronouns work in any context, referring to the last-mentioned object.

**Player experience:**
```
> EXAMINE SKULL
Lying in one corner of the room is a beautifully carved crystal skull...

> TAKE IT
Taken.
```

### Example 4: ME (Self-Reference)

From [gglobals.zil:214-265](gglobals.zil#L214-L265):

```python
class ME(Object):
    """Self-reference object"""
    location = GLOBAL_OBJECTS
    synonyms = ["ME", "MYSELF", "SELF", "CRETIN"]
    description = "you"
    flags = [ACTORBIT]
    action = cretin_fcn

def cretin_fcn():
    """Handle self-referential commands"""
    if current_verb == "TELL":
        print("Talking to yourself is said to be a sign of impending mental collapse.")

    elif current_verb == "EXAMINE":
        if HERE in [MIRROR_1.location, MIRROR_2.location]:
            print("Your image in the mirror looks tired.")
        else:
            print("That's difficult unless your eyes are prehensile.")

    elif current_verb == "ATTACK":
        if PRSI and PRSI.has_flag(WEAPONBIT):
            jigs_up("If you insist.... Poof, you're dead!")
        else:
            print("Suicide is not the answer.")

    elif current_verb == "EAT":
        print("Auto-cannibalism is not the answer.")
```

**Why it's global:** Players can reference themselves from anywhere with humorous results.

**Player experience:**
```
> EXAMINE ME
That's difficult unless your eyes are prehensile.

> ATTACK ME
Suicide is not the answer.

> TELL MYSELF ABOUT THE TREASURE
Talking to yourself is said to be a sign of impending mental collapse.
```

---

## LOCAL-GLOBALS (Conditionally Visible)

Objects in LOCAL-GLOBALS are only visible in **specific rooms** that declare them in their GLOBAL property.

### Definition:

From [gglobals.zil:12-24](gglobals.zil#L12-L24):

```python
class LOCAL_GLOBALS(Object):
    """Container for room-specific scenery objects"""
    location = GLOBAL_OBJECTS  # Nested inside global objects
    synonyms = ["ZZMGCK"]  # Internal reference
    descfcn = path_object
    global_ref = GLOBAL_OBJECTS
```

### Complete List of Local-Global Objects:

From [1dungeon.zil](1dungeon.zil):

```python
# Scenery
BOARD           # Wooden boards (boarded windows)
SONGBIRD        # Bird in trees
WHITE_HOUSE     # The white house building
FOREST          # Forest scenery
TREE            # Trees in forest
GLOBAL_WATER    # Water/stream in various locations
SLIDE           # Coal chute slide

# Interactive Objects
KITCHEN_WINDOW  # Kitchen window (can open/close)
CHIMNEY         # Chimney (can climb)
STAIRS          # Stairs in various locations

# Room-Specific
PSEUDO_OBJECT   # Pseudo-object handler
```

### How Rooms Declare Local-Globals:

```python
class KITCHEN(Room):
    description = "Kitchen"
    # Declare which local-global objects are visible here
    global_objects = [KITCHEN_WINDOW, CHIMNEY, STAIRS]
    exits = {
        WEST: LIVING_ROOM,
        EAST: ConditionalExit(EAST_OF_HOUSE,
                             condition=lambda: KITCHEN_WINDOW.has_flag(OPENBIT))
    }
```

### Example 1: KITCHEN-WINDOW

From [1dungeon.zil:93-99](1dungeon.zil#L93-L99):

```python
class KITCHEN_WINDOW(Object):
    """Window - only visible in kitchen and east of house"""
    location = LOCAL_GLOBALS
    synonyms = ["WINDOW"]
    adjectives = ["KITCHEN", "SMALL"]
    description = "window"
    flags = [NDESCBIT, DOORBIT]  # Acts as a door
    action = kitchen_window_fcn

def kitchen_window_fcn():
    """Handle window interactions"""
    if current_verb == "OPEN":
        if HERE == EAST_OF_HOUSE:
            KITCHEN_WINDOW.add_flag(OPENBIT)
            print("With great effort, you open the window far enough to allow entry.")
        else:
            print("The window is now open.")

    elif current_verb == "CLOSE":
        KITCHEN_WINDOW.remove_flag(OPENBIT)
        print("The window is closed.")
```

**Rooms that see this object:**
```python
# From 1dungeon.zil:1304-1307
class EAST_OF_HOUSE(Room):
    global_objects = [WHITE_HOUSE, KITCHEN_WINDOW, FOREST, BOARD]

# From 1dungeon.zil:1441
class KITCHEN(Room):
    global_objects = [KITCHEN_WINDOW, CHIMNEY, STAIRS]
```

**Why it's local-global:** The window exists in two specific rooms, not everywhere.

**Player experience:**
```
> (at EAST-OF-HOUSE)
> OPEN WINDOW
With great effort, you open the window far enough to allow entry.

> WEST
Kitchen
This is the kitchen of the white house.

> CLOSE WINDOW
The window is closed.
```

### Example 2: STAIRS

From [gglobals.zil:102-113](gglobals.zil#L102-L113):

```python
class STAIRS(Object):
    """Stairs - visible in multiple rooms with stairs"""
    location = LOCAL_GLOBALS
    synonyms = ["STAIRS", "STEPS", "STAIRCASE", "STAIRWAY"]
    adjectives = ["STONE", "DARK", "MARBLE", "FORBIDDING", "STEEP"]
    description = "stairs"
    flags = [NDESCBIT, CLIMBBIT]
    action = stairs_f

def stairs_f():
    """Handle stairs interactions"""
    if current_verb == "THROUGH":
        print("You should say whether you want to go up or down.")
```

**Rooms that see this object:**
```python
# Multiple rooms declare STAIRS as visible
LIVING_ROOM.global_objects = [STAIRS]
KITCHEN.global_objects = [KITCHEN_WINDOW, CHIMNEY, STAIRS]
CELLAR.global_objects = [TRAP_DOOR, SLIDE, STAIRS]
TORCH_ROOM.global_objects = [STAIRS]
# ... and more
```

**Why it's local-global:** Many rooms have stairs, but not all. Each room opts in.

**Player experience:**
```
> EXAMINE STAIRS
The stairs are made of stone.

> GO THROUGH STAIRS
You should say whether you want to go up or down.

> UP
(goes up the stairs)
```

### Example 3: WHITE-HOUSE

From [1dungeon.zil:48-54](1dungeon.zil#L48-L54):

```python
class WHITE_HOUSE(Object):
    """The white house building - visible from outside"""
    location = LOCAL_GLOBALS
    synonyms = ["HOUSE", "HOME"]
    adjectives = ["WHITE", "BEAUTIFUL", "COLONIAL"]
    description = "white house"
    flags = [NDESCBIT]
    action = white_house_fcn

# Visible in these outdoor rooms:
WEST_OF_HOUSE.global_objects = [WHITE_HOUSE, BOARD, FOREST]
NORTH_OF_HOUSE.global_objects = [WHITE_HOUSE, FOREST]
SOUTH_OF_HOUSE.global_objects = [WHITE_HOUSE, FOREST]
EAST_OF_HOUSE.global_objects = [WHITE_HOUSE, KITCHEN_WINDOW, FOREST, BOARD]
```

**Why it's local-global:** The house is visible from outside, but not from inside or underground.

**Player experience:**
```
> (at WEST-OF-HOUSE)
> EXAMINE HOUSE
The house is a beautiful colonial house which is painted white.

> WEST
Forest

> EXAMINE HOUSE
You can't see any house here!
```

### Example 4: FOREST

From [1dungeon.zil:56-61](1dungeon.zil#L56-L61):

```python
class FOREST(Object):
    """Forest scenery - visible in outdoor areas"""
    location = LOCAL_GLOBALS
    synonyms = ["FOREST", "TREES", "WOODS"]
    adjective = ["DENSE"]
    description = "forest"
    flags = [NDESCBIT]

# Visible in outdoor forest rooms
WEST_OF_HOUSE.global_objects = [WHITE_HOUSE, BOARD, FOREST]
NORTH_OF_HOUSE.global_objects = [WHITE_HOUSE, FOREST]
FOREST_1.global_objects = [FOREST]
FOREST_2.global_objects = [FOREST]
# etc.
```

**Why it's local-global:** Forest is visible in forest areas, but not in house or underground.

---

## How the Parser Finds Objects

The parser searches for objects in this order:

```python
def find_object(word, location):
    """Search for object matching word"""

    # 1. Check inventory
    for obj in WINNER.children:
        if word in obj.synonyms:
            return obj

    # 2. Check current room
    for obj in HERE.children:
        if word in obj.synonyms:
            return obj

    # 3. Check GLOBAL-OBJECTS (always visible)
    for obj in GLOBAL_OBJECTS.children:
        if word in obj.synonyms:
            return obj

    # 4. Check LOCAL-GLOBALS (if room declares them)
    for obj in LOCAL_GLOBALS.children:
        if word in obj.synonyms:
            # Check if current room has this object in GLOBAL list
            if obj in HERE.global_objects:
                return obj

    # 5. Check open containers in room
    for container in HERE.children:
        if container.has_flag(CONTBIT) and container.has_flag(OPENBIT):
            for obj in container.children:
                if word in obj.synonyms:
                    return obj

    # Not found
    return NOT_HERE_OBJECT
```

---

## Summary Table

| Type | Visibility | Use Case | Example |
|------|------------|----------|---------|
| **GLOBAL-OBJECTS** | All rooms | Universal scenery, abstract concepts, persistent NPCs | GRUE, GROUND, IT, HANDS, ME |
| **LOCAL-GLOBALS** | Specific rooms (declared in GLOBAL property) | Multi-room scenery, region-specific objects | KITCHEN-WINDOW, STAIRS, WHITE-HOUSE, FOREST |
| **Regular Objects** | Single location | Normal items, unique objects | LAMP, SWORD, SKULL, BOTTLE |

---

## Benefits of This System

1. **Memory Efficiency**: Define once, reference many times
2. **Consistency**: Same object logic in all locations
3. **Maintainability**: Change once, affects everywhere
4. **Flexibility**: Rooms opt into local-globals as needed
5. **Realism**: Scenery exists without cluttering room descriptions
6. **Code Reuse**: Single action handler for all instances

## Design Pattern Example

Instead of duplicating objects:

```python
# BAD: Duplicating ground in every room
class KITCHEN(Room):
    children = [KITCHEN_GROUND, TABLE, BOTTLE]

class CELLAR(Room):
    children = [CELLAR_GROUND, STAIRS]

# Each needs its own ground object with duplicate code!
```

Using globals:

```python
# GOOD: One ground object, visible everywhere
class GROUND(Object):
    location = GLOBAL_OBJECTS
    # Define once, works everywhere

class KITCHEN(Room):
    children = [TABLE, BOTTLE]
    # No need to add ground

class CELLAR(Room):
    children = [STAIRS]
    # Ground is automatically visible
```

This pattern made Zork's 110-room world manageable in 1980s memory constraints!
