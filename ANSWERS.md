# ANSWERS TO .todo.claude.md QUESTIONS

## 1. List of Verbs

Zork I contains **267 verbs** (including synonyms and variants). Here's a categorized breakdown:

### Navigation/Movement (40+ verbs)
NORTH, SOUTH, EAST, WEST, DOWN, UP, NE, NW, SE, SW, WALK, RUN, GO, PROCEED, STEP, BACK, ENTER, EXIT, LEAVE, JUMP, LEAP, DIVE, CLIMB, CLIMB-UP, CLIMB-DOWN, CLIMB-ON, CLIMB-IN, CROSS, FORD, FOLLOW, PURSUE, CHASE, COME, WAIT, STAND, SWIM, BATHE, WADE

### Object Interaction (50+ verbs)
TAKE, GET, HOLD, CARRY, REMOVE, GRAB, CATCH, PICK-UP, DROP, PUT-DOWN, OPEN, CLOSE, EXAMINE, DESCRIBE, WHAT, LOOK-AT, LOOK, STARE, GAZE, READ, SKIM, LOOK-INSIDE, LOOK-UNDER, LOOK-BEHIND, LOOK-ON, SEARCH, FIND, WHERE, SEEK, SEE, COUNT, SHAKE, PUT, STUFF, INSERT, PLACE, HIDE, MOVE, ROLL, PULL, TUG, YANK, PUSH, PRESS, RAISE, LIFT, LOWER, TURN, SET, FLIP, KICK

### Container/Content Actions (15+ verbs)
FILL, POUR, SPILL, DRINK, IMBIBE, SWALLOW, DRINK-FROM, EAT, CONSUME, TASTE, BITE, PLAY, RING, PEAL, SQUEEZE

### Combat/Interaction (30+ verbs)
ATTACK, FIGHT, HURT, INJURE, HIT, KILL, MURDER, SLAY, DISPATCH, STAB, STRIKE, SWING, THRUST, THROW, HURL, CHUCK, TOSS, THROW-AT, CUT, SLICE, PIERCE, POKE, PUNCTURE, TIE, FASTEN, UNTIE, FREE, RELEASE, KISS, COMMAND

### Magical/Mystical (20+ verbs)
ACTIVATE, LIGHT, TURN-ON, TURN-OFF, EXTINGUISH, DOUSE, BLOW-OUT, BLOW-UP, BLOW-IN, BREATHE, BURN, INCINERATE, IGNITE, DEFLATE, INFLATE, PUMP, INCANT, CHANT, ENCHANT, DISENCHANT, EXORCISE, BANISH, PLUGH, XYZZY, FROBOZZ, ZORK

### Communication (20+ verbs)
SAY, HELLO, HI, TELL, ASK, TALK-TO, ANSWER, REPLY, YELL, SCREAM, SHOUT, CURSE, SHIT, FUCK, DAMN, PRAY, REPENT, MUMBLE, SIGH, WISH, SEND-FOR

### Tool Use (15+ verbs)
BRUSH, CLEAN, DIG, LOCK, UNLOCK, WIND, WIND-UP, PICK, OIL, LUBRICATE, GREASE, WEAR, APPLY, DESTROY, DAMAGE, BREAK, SMASH

### Vehicle/Transport (5+ verbs)
BOARD, DISEMBARK, LAUNCH, LAND

### Game Commands (20+ verbs)
VERBOSE, BRIEF, SUPERBRIEF, DIAGNOSE, INVENTORY, I, QUIT, Q, RESTART, RESTORE, SAVE, SCORE, SCRIPT, UNSCRIPT, VERSION, VERIFY

---

## 2. List of NPCs

Zork I contains **5 NPCs** with the ACTORBIT flag:

### 1. GHOSTS
- **Location:** Entrance to Hades
- **Description:** "number of ghosts"
- **Adjectives:** INVISIBLE, EVIL
- **Synonyms:** SPIRITS, FIENDS, FORCE
- **Special Behavior:** GHOSTS-F action routine
- **Combat:** Cannot be attacked directly, must be exorcised using bell, book, and candles ritual

### 2. BAT
- **Location:** Bat Room
- **Description:** "bat"
- **Adjectives:** VAMPIRE, DERANGED
- **Synonyms:** VAMPIRE
- **Special Behavior:** BAT-F action routine with custom description (BAT-D)
- **Combat:** Flies around randomly, can be dangerous

### 3. CYCLOPS
- **Location:** Cyclops Room
- **Description:** "cyclops"
- **Adjectives:** HUNGRY, GIANT
- **Synonyms:** MONSTER, EYE
- **Strength:** 10,000 (nearly invincible!)
- **Special Behavior:** CYCLOPS-FCN action routine
- **Combat:** Extremely dangerous - requires puzzle solution (say "ODYSSEUS" or give lunch+water)

### 4. TROLL
- **Location:** Troll Room
- **Description:** "A nasty-looking troll, brandishing a bloody axe, blocks all passages out of the room."
- **Adjectives:** NASTY
- **Strength:** 2
- **Special Behavior:** TROLL-FCN action routine
- **Weapon:** Bloody axe
- **Combat:** Must be defeated or tricked with food to open passages

### 5. THIEF
- **Location:** Round Room (appears randomly)
- **Description:** "There is a suspicious-looking individual, holding a large bag, leaning against one wall. He is armed with a deadly stiletto."
- **Adjectives:** SHADY, SUSPICIOUS, SEEDY
- **Synonyms:** ROBBER, MAN, PERSON
- **Strength:** 5
- **Weapon:** Stiletto
- **Special Behavior:** ROBBER-FUNCTION action routine
- **Special:** Steals treasures randomly, has INVISIBLE flag initially, carries CONTBIT (bag)

---

## 3. List of Items (Organized by Category)

### Weapons (6 items)
1. **SWORD** - Elvish sword (Living Room) - WEAPONBIT, glows near evil
2. **AXE** - Bloody axe (Troll's weapon) - WEAPONBIT
3. **KNIFE** - Nasty knife (Attic Table) - WEAPONBIT
4. **RUSTY-KNIFE** - Cursed knife (Maze-5) - WEAPONBIT, TOOLBIT
5. **STILETTO** - Vicious stiletto (Thief's weapon) - WEAPONBIT
6. **SCEPTRE** - Sharp Egyptian sceptre (in Coffin) - WEAPONBIT (also treasure, value: 6)

### Treasures (20+ items with TVALUE > 0)
1. **SKULL** - Crystal skull (Land of Living Dead) - Value: 10
2. **DIAMOND** - Huge diamond - Valuable treasure
3. **JADE** - Exquisite jade figurine (Bat Room) - Treasure
4. **TRIDENT** - Crystal trident of Poseidon (Atlantis) - Value: 4, TValue: 11
5. **EMERALD** - Large emerald (in Buoy) - Value: 5, TValue: 10
6. **CHALICE** - Silver chalice (Treasure Room) - CONTBIT
7. **PAINTING** - Beautiful painting (Gallery) - BURNBIT
8. **BRACELET** - Sapphire-encrusted bracelet (Gas Room) - Value: 5, TValue: 5
9. **CANARY** - Golden clockwork canary (in Egg) - Value: 6, TValue: 4
10. **EGG** - Jewel-encrusted egg (in Nest) - Value: 5, TValue: 5, CONTBIT
11. **BAUBLE** - Beautiful brass bauble - Value: 1, TValue: 1
12. **BAR** - Platinum bar (Loud Room) - Value: 10, TValue: 5, SACREDBIT
13. **POT-OF-GOLD** - (End of Rainbow) - Value: 10, TValue: 10
14. **SCARAB** - Jeweled scarab (Sandy Cave) - Value: 5, TValue: 5
15. **TORCH** - Flaming ivory torch (Pedestal) - Value: 14, TValue: 6, LIGHTBIT
16. **TRUNK** - Trunk of jewels (Reservoir) - Value: 15, TValue: 5
17. **BAG-OF-COINS** - Leather bag (Maze-5) - Value: 10, TValue: 5
18. **COFFIN** - Gold coffin (Egypt Room) - CONTBIT, SACREDBIT (also container)
19. **SCEPTRE** - Egyptian sceptre (in Coffin) - Value: 4, TValue: 6

### Light Sources (4 items)
1. **LAMP** - Brass lantern (Living Room) - LIGHTBIT, battery-powered
2. **TORCH** - Flaming ivory torch (Pedestal) - FLAMEBIT, ONBIT, LIGHTBIT
3. **CANDLES** - Burning pair (South Temple) - FLAMEBIT, ONBIT, LIGHTBIT
4. **BURNED-OUT-LANTERN** - Useless dead lantern (Maze-5)

### Food & Drink (4 items)
1. **WATER** - (in Bottle) - DRINKBIT
2. **LUNCH** - Hot pepper sandwich (in Sandwich-Bag) - FOODBIT
3. **GARLIC** - Clove of garlic (in Sandwich-Bag) - FOODBIT
4. **GUNK** - Small vitreous slag - Special item

### Tools (10+ items)
1. **PUMP** - Hand-held air pump (Reservoir-North) - TOOLBIT
2. **SCREWDRIVER** - Screw driver (Maintenance Room) - TOOLBIT
3. **KEYS** - Skeleton key (Maze-5) - TOOLBIT
4. **SHOVEL** - (Sandy Beach) - TOOLBIT
5. **WRENCH** - (Maintenance Room) - TOOLBIT
6. **PUTTY** - Viscous material (in Tube) - TOOLBIT
7. **RUSTY-KNIFE** - (Maze-5) - TOOLBIT, WEAPONBIT
8. **ROPE** - Large coil of rope (Attic) - SACREDBIT
9. **BELL** - Brass bell (North Temple) - For exorcism ritual
10. **BOOK** - Large black book (Altar) - READBIT, BURNBIT, for exorcism

### Containers (15+ items)
1. **BOTTLE** - Glass bottle (Kitchen Table) - TRANSBIT, CONTBIT
2. **CHALICE** - Silver chalice (Treasure Room) - CONTBIT
3. **SANDWICH-BAG** - Brown sack (Kitchen Table) - CONTBIT, BURNBIT
4. **COFFIN** - Gold coffin (Egypt Room) - CONTBIT, SACREDBIT
5. **BUOY** - Red buoy (River-4) - CONTBIT
6. **EGG** - Jewel-encrusted (Nest) - CONTBIT, SEARCHBIT
7. **BROKEN-EGG** - Broken egg - CONTBIT, OPENBIT
8. **TUBE** - Toothpaste tube (Maintenance Room) - CONTBIT, READBIT
9. **NEST** - Bird's nest (Up-a-Tree) - BURNBIT, CONTBIT, OPENBIT
10. **LARGE-BAG** - Large bag (in Thief) - CONTBIT
11. **MACHINE** - PDP-10/Dryer (Machine Room) - CONTBIT

### Readable Items (10+ items)
1. **BOOK** - Large black book (Altar) - READBIT
2. **ADVERTISEMENT** - Leaflet (Mailbox) - READBIT, BURNBIT
3. **MATCH** - Matchbook (Dam Lobby) - READBIT
4. **GUIDE** - Tour guidebook (Dam Lobby) - READBIT, BURNBIT
5. **OWNERS-MANUAL** - Zork manual (Studio) - READBIT
6. **MAP** - Ancient map (Trophy Case) - READBIT, INVISIBLE
7. **BOAT-LABEL** - Tan label (Inflated-Boat) - READBIT, BURNBIT
8. **TUBE** - Toothpaste tube - READBIT

### Vehicles (3 items)
1. **INFLATED-BOAT** - Magic boat - VEHBIT, OPENBIT
2. **INFLATABLE-BOAT** - Plastic pile (Dam Base) - BURNBIT
3. **PUNCTURED-BOAT** - Punctured boat - BURNBIT

### Burnables (10+ items)
1. **LEAVES** - Pile of leaves (Grating Clearing) - BURNBIT
2. **COAL** - Small pile of coal (Dead-End-5) - BURNBIT
3. **PAINTING** - Beautiful painting - BURNBIT
4. **SANDWICH-BAG** - Brown sack - BURNBIT
5. **NEST** - Bird's nest - BURNBIT
6. **ADVERTISEMENT** - Leaflet - BURNBIT
7. **GUIDE** - Tour guidebook - BURNBIT
8. **BOAT-LABEL** - Tan label - BURNBIT
9. **BOOK** - Large black book - BURNBIT
10. **INFLATABLE-BOAT** - Plastic pile - BURNBIT

### Miscellaneous (10+ items)
1. **MIRROR-1** - Mirror (Mirror-Room-1) - TRYTAKEBIT
2. **MIRROR-2** - Mirror (Mirror-Room-2) - TRYTAKEBIT
3. **TIMBERS** - Broken timbers (Timber Room)
4. **RAINBOWFCN** - Rainbow climbing object - CLIMBBIT
5. **HOT-BELL** - Red hot brass bell - TRYTAKEBIT
6. **BROKEN-CANARY** - Broken clockwork - TValue: 1

**Total Takeable Items:** 80+

---

## 4. Special Behaviors on Items

The following items have custom ACTION routines that provide unique behaviors:

### Combat & Weapon Behaviors
- **SWORD-FCN** - Elvish sword glows blue near evil creatures (Troll)
- **AXE-F** - Bloody axe weapon properties
- **KNIFE-F** - Nasty knife weapon
- **RUSTY-KNIFE-FCN** - Cursed knife - causes suicide if used to attack yourself!
- **STILETTO-FUNCTION** - Thief's weapon

### Light Source Behaviors
- **LANTERN** - Brass lantern has limited battery life (200 turns), gives warnings as power depletes
- **CANDLES-FCN** - Burning candles burn for ~40 turns before going out
- **TORCH** - Eternally burning, cannot be extinguished

### Treasure Behaviors
- **SCEPTRE-FUNCTION** - Wave at rainbow to solidify it, allowing crossing to pot of gold
- **PAINTING-FCN** - Can be burned or damaged, reducing its value to 0
- **EGG-OBJECT** - Fragile! Breaks if dropped, releasing canary inside
- **CANARY-OBJECT** - Wind in forest to attract songbird, which takes canary and drops bauble
- **TRUNK-F** - Treasure trunk only accessible when reservoir is drained
- **BAG-OF-COINS-F** - Cannot be opened, tied shut

### Container Behaviors
- **BOTTLE-FUNCTION** - Glass bottle can hold water, must be opened to drink from
- **CHALICE-FCN** - Silver chalice special handling
- **SANDWICH-BAG-FCN** - Contains hot pepper lunch and garlic
- **TUBE-FUNCTION** - Toothpaste tube contains putty for boat repair
- **MACHINE-F** - Complex PDP-10/dryer puzzle with buttons and switches

### Environmental/Puzzle Item Behaviors
- **BELL-F** - Brass bell used in exorcism ritual (bell, book, and candle)
- **HOT-BELL-F** - Bell becomes too hot to touch after certain actions
- **BLACK-BOOK** - Prayer book, readable, used in exorcism ritual, burning it is fatal!
- **ROPE-FUNCTION** - Can be tied to railing and climbed down in dome room
- **LEAF-PILE** - Moving leaves reveals grating
- **PUTTY-FCN** - Viscous material repairs punctured boat
- **MATCH-FUNCTION** - Limited matches (20), each burns for 2 turns

### Vehicle Behaviors
- **RBOAT-FUNCTION** - Inflated boat allows river navigation
- **IBOAT-FUNCTION** - Inflate with pump to create working boat
- **DBOAT-FUNCTION** - Punctured boat sinks, needs putty repair
- **RAINBOW-FCN** - Rainbow becomes climbable when sceptre is waved

### Food & Consumable Behaviors
- **WATER-F** - Water can be drunk, poured, given to cyclops
- **GARLIC-F** - Garlic has vampire-repelling properties (for bat?)
- **LUNCH** - Hot pepper sandwich makes cyclops thirsty
- **GUNK-FUNCTION** - Vitreous slag special interaction

### Special Purpose Item Behaviors
- **MIRROR-MIRROR** - Mirrors reflect player appearance, cannot be taken
- **LARGE-BAG-F** - Thief's bag for holding stolen items
- **RUG-FCN** - Moving rug reveals trap door in living room
- **TROPHY-CASE-FCN** - Placing treasures inside scores points

### Environmental Object Behaviors (not takeable but notable)
- **DAM-FUNCTION** - Flood control dam can be operated
- **BOLT-F** - Turning bolt opens/closes dam gates
- **BUTTON-F** - Four buttons in maintenance room control dam
- **GRATE-FUNCTION** - Grating requires skeleton key to unlock
- **KITCHEN-WINDOW-F** - Window can be opened to enter house
- **CHIMNEY-F** - Chimney allows climbing up from studio to kitchen
- **CRACK-FCN** - Crack/crevice mechanics
- **BASKET-F** - Dumbwaiter/cage for vertical transport

---

## 5. Python Pseudocode: LOCAL-GLOBALS vs Direct Room Declaration

### Question
Why do you need to declare an object in LOCAL-GLOBALS to have it in different rooms instead of declaring it directly in each room? Does it make sense in modern Python as opposed to ZIL?

### Answer

The LOCAL-GLOBALS system in ZIL solves the **"shared scenery object" problem** - how to make the same object (like stairs, windows, or walls) appear in multiple rooms without code duplication.

### The Problem: Without LOCAL-GLOBALS

#### BAD: Direct Declaration in Each Room (Code Duplication)

```python
class Kitchen:
    def __init__(self):
        self.name = "Kitchen"
        self.exits = {"west": "living_room"}

        # Declare window directly in this room
        self.kitchen_window = Object(
            name="window",
            description="small window",
            can_open=True,
            action=self.window_action
        )

        # Declare stairs directly in this room
        self.stairs = Object(
            name="stairs",
            description="wooden stairs",
            can_climb=True,
            action=self.stairs_action
        )

    def window_action(self, verb):
        if verb == "open":
            print("Window opened")
            # Open window in THIS room
            self.kitchen_window.is_open = True
        elif verb == "close":
            print("Window closed")
            self.kitchen_window.is_open = False

    def stairs_action(self, verb):
        if verb == "climb":
            print("You should specify UP or DOWN")

class LivingRoom:
    def __init__(self):
        self.name = "Living Room"
        self.exits = {"east": "kitchen"}

        # DUPLICATE: Must re-declare stairs here!
        self.stairs = Object(
            name="stairs",
            description="wooden stairs",
            can_climb=True,
            action=self.stairs_action  # DUPLICATE CODE!
        )

    def stairs_action(self, verb):
        # SAME CODE AS KITCHEN!
        if verb == "climb":
            print("You should specify UP or DOWN")

class Cellar:
    def __init__(self):
        self.name = "Cellar"

        # DUPLICATE AGAIN!
        self.stairs = Object(
            name="stairs",
            description="stone stairs",  # Different description!
            can_climb=True,
            action=self.stairs_action  # DUPLICATE CODE!
        )

    def stairs_action(self, verb):
        # SAME CODE AGAIN!
        if verb == "climb":
            print("You should specify UP or DOWN")
```

**Problems:**
1. ❌ **Code duplication** - Same `stairs_action` in 3 places
2. ❌ **Inconsistency risk** - Each room might implement stairs differently
3. ❌ **Memory waste** - Three separate stair objects instead of one
4. ❌ **Hard to maintain** - Bug fix requires changing 3 places
5. ❌ **State inconsistency** - If kitchen window opens, should it be open in other rooms?

---

### The Solution: LOCAL-GLOBALS Pattern

#### GOOD: Shared Object with Room Opt-In

```python
# Global container for shared objects
class LocalGlobals:
    """Container for objects that appear in multiple rooms"""
    def __init__(self):
        self.objects = {}

    def register(self, name, obj):
        self.objects[name] = obj

    def get(self, name):
        return self.objects.get(name)

# Create global instance
LOCAL_GLOBALS = LocalGlobals()

# Define KITCHEN_WINDOW once
class KitchenWindow:
    def __init__(self):
        self.name = "window"
        self.synonyms = ["window"]
        self.description = "small kitchen window"
        self.is_open = False

    def action(self, verb, current_room):
        if verb == "open":
            if current_room == "east_of_house":
                print("With great effort, you open the window far enough to allow entry.")
            else:
                print("The window is now open.")
            self.is_open = True

        elif verb == "close":
            print("The window is closed.")
            self.is_open = False

# Define STAIRS once
class Stairs:
    def __init__(self):
        self.name = "stairs"
        self.synonyms = ["stairs", "steps", "staircase"]
        self.adjectives = ["stone", "wooden", "marble"]
        self.description = "stairs"

    def action(self, verb):
        if verb == "climb":
            print("You should say whether you want to go up or down.")
        elif verb == "examine":
            print("Just ordinary stairs.")

# Register shared objects
kitchen_window = KitchenWindow()
stairs = Stairs()

LOCAL_GLOBALS.register("kitchen_window", kitchen_window)
LOCAL_GLOBALS.register("stairs", stairs)

# Rooms declare which local-globals they use
class Kitchen:
    def __init__(self):
        self.name = "Kitchen"
        self.exits = {"west": "living_room"}

        # Declare which local-global objects are visible here
        self.local_globals = ["kitchen_window", "stairs"]

        # Also has local objects specific to this room
        self.table = Object(name="table", description="wooden table")

    def get_visible_objects(self):
        """Return all objects visible in this room"""
        # Local objects
        objects = [self.table]

        # Add local-global objects
        for global_name in self.local_globals:
            obj = LOCAL_GLOBALS.get(global_name)
            if obj:
                objects.append(obj)

        return objects

class LivingRoom:
    def __init__(self):
        self.name = "Living Room"
        self.exits = {"east": "kitchen"}

        # Only stairs visible here, not window
        self.local_globals = ["stairs"]

        self.rug = Object(name="rug", description="oriental rug")

    def get_visible_objects(self):
        objects = [self.rug]
        for global_name in self.local_globals:
            obj = LOCAL_GLOBALS.get(global_name)
            if obj:
                objects.append(obj)
        return objects

class EastOfHouse:
    def __init__(self):
        self.name = "East of House"

        # Window visible from outside too!
        self.local_globals = ["kitchen_window"]

    def get_visible_objects(self):
        objects = []
        for global_name in self.local_globals:
            obj = LOCAL_GLOBALS.get(global_name)
            if obj:
                objects.append(obj)
        return objects

class Cellar:
    def __init__(self):
        self.name = "Cellar"

        # Stairs visible here too
        self.local_globals = ["stairs"]

    def get_visible_objects(self):
        objects = []
        for global_name in self.local_globals:
            obj = LOCAL_GLOBALS.get(global_name)
            if obj:
                objects.append(obj)
        return objects
```

---

### How the Parser Finds Objects

```python
class Parser:
    def __init__(self):
        self.current_room = None

    def find_object(self, word):
        """
        Search for object matching word

        Search order:
        1. Player's inventory
        2. Room's local objects
        3. Room's local-global objects
        4. Global objects (everywhere)
        """

        # 1. Check inventory
        for obj in PLAYER.inventory:
            if word in obj.synonyms:
                return obj

        # 2. Check room's local objects
        for obj in self.current_room.get_local_objects():
            if word in obj.synonyms:
                return obj

        # 3. Check room's local-global objects
        for global_name in self.current_room.local_globals:
            obj = LOCAL_GLOBALS.get(global_name)
            if obj and word in obj.synonyms:
                return obj

        # 4. Check global objects (always visible)
        for obj in GLOBAL_OBJECTS:
            if word in obj.synonyms:
                return obj

        # Not found
        return None
```

---

### Benefits of LOCAL-GLOBALS Pattern

| Aspect | Direct Declaration | LOCAL-GLOBALS Pattern |
|--------|-------------------|----------------------|
| **Code Reuse** | ❌ Duplicate code in each room | ✅ Define once, use many times |
| **Consistency** | ❌ Each room might differ | ✅ Same behavior everywhere |
| **Memory** | ❌ Multiple object instances | ✅ Single shared instance |
| **Maintainability** | ❌ Fix bugs in N places | ✅ Fix once, applies everywhere |
| **State Management** | ❌ Each room has separate state | ✅ Shared state across rooms |
| **Flexibility** | ❌ Hard to control visibility | ✅ Rooms opt-in to what they see |

---

### Does This Make Sense in Modern Python?

**YES!** The LOCAL-GLOBALS pattern is still valuable in modern Python for:

1. **Game Development** - Shared scenery objects in multiple rooms
2. **UI Components** - Reusable widgets appearing in different views
3. **Configuration** - Shared resources with scoped visibility
4. **Resource Management** - Singleton objects with conditional access

**Modern Python Alternative: Dependency Injection**

```python
from typing import List, Dict

class Room:
    def __init__(self, name: str, shared_objects: List[str]):
        self.name = name
        self.shared_objects = shared_objects

        # Inject shared object references
        self._shared_registry = LOCAL_GLOBALS

    def get_object(self, name: str):
        # Check shared objects first
        if name in self.shared_objects:
            return self._shared_registry.get(name)
        return None

# Usage
kitchen = Room("Kitchen", shared_objects=["kitchen_window", "stairs"])
living_room = Room("Living Room", shared_objects=["stairs"])
cellar = Room("Cellar", shared_objects=["stairs"])
```

**Even More Modern: Python Data Classes**

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class SharedObject:
    name: str
    description: str
    action: callable

@dataclass
class Room:
    name: str
    shared_objects: List[str] = field(default_factory=list)

    def has_object(self, obj_name: str) -> bool:
        return obj_name in self.shared_objects

# Shared registry
SHARED_OBJECTS = {
    "stairs": SharedObject("stairs", "wooden stairs", lambda: print("Climb up or down?")),
    "window": SharedObject("window", "kitchen window", lambda: print("Window action"))
}

kitchen = Room("Kitchen", shared_objects=["window", "stairs"])
living_room = Room("Living Room", shared_objects=["stairs"])
```

---

### Complete Example: Kitchen, Table, Stairs, Window

```python
# Shared object registry
class LocalGlobals:
    def __init__(self):
        self.objects = {}

    def register(self, name, obj):
        self.objects[name] = obj

    def get(self, name):
        return self.objects.get(name)

LOCAL_GLOBALS = LocalGlobals()

# Define shared objects ONCE
class Stairs:
    def __init__(self):
        self.name = "stairs"

    def action(self, verb):
        if verb == "climb":
            return "You should say whether you want to go up or down."
        return "Just ordinary stairs."

class KitchenWindow:
    def __init__(self):
        self.name = "window"
        self.is_open = False

    def action(self, verb):
        if verb == "open":
            self.is_open = True
            return "Window opened."
        elif verb == "close":
            self.is_open = False
            return "Window closed."
        return "It's a window."

# Register shared objects
stairs = Stairs()
kitchen_window = KitchenWindow()

LOCAL_GLOBALS.register("stairs", stairs)
LOCAL_GLOBALS.register("kitchen_window", kitchen_window)

# Define rooms with local-global references
class Kitchen:
    def __init__(self):
        self.name = "Kitchen"

        # Local object (only in kitchen)
        self.table = {"name": "table", "description": "wooden table"}

        # Declare which shared objects are visible
        self.local_globals = ["kitchen_window", "stairs"]

    def get_objects(self):
        objects = [self.table]
        for name in self.local_globals:
            obj = LOCAL_GLOBALS.get(name)
            if obj:
                objects.append(obj)
        return objects

class LivingRoom:
    def __init__(self):
        self.name = "Living Room"

        # Only stairs visible (not window)
        self.local_globals = ["stairs"]

    def get_objects(self):
        objects = []
        for name in self.local_globals:
            obj = LOCAL_GLOBALS.get(name)
            if obj:
                objects.append(obj)
        return objects

# Usage
kitchen = Kitchen()
living_room = LivingRoom()

print("Kitchen objects:", [obj.name if hasattr(obj, 'name') else obj['name']
                          for obj in kitchen.get_objects()])
# Output: ['table', 'window', 'stairs']

print("Living Room objects:", [obj.name for obj in living_room.get_objects()])
# Output: ['stairs']

# State is shared
print(stairs.action("climb"))  # Same object in both rooms
# Output: "You should say whether you want to go up or down."
```

---

### Conclusion

The LOCAL-GLOBALS pattern **absolutely makes sense in modern Python**! It provides:

✅ **Single source of truth** - Define once, use many times
✅ **Shared state** - Window opened in kitchen stays open everywhere
✅ **Flexible visibility** - Rooms control what they show
✅ **Memory efficiency** - One object instance instead of many
✅ **Maintainability** - Fix bugs once, applies everywhere

ZIL's LOCAL-GLOBALS is a **clever design pattern** that translates well to modern programming, demonstrating that good architecture transcends language and era!

---

## 6. Analysis of Provided Documentation

### Themes Covered in the Documentation Files

Based on review of **GLOBAL_OBJECTS.md**, **GRAPHS.md**, **OBJECT_LOCATIONS.md**, **PUZZLES.md**, **ROOM_ANALISYS.md** (MAP.md), and **SYNTAXRULE.md**, the following themes are comprehensively covered:

#### ✅ Well-Covered Topics:

1. **Object System Architecture**
   - Global objects vs local-globals
   - Object hierarchy and containment
   - Object flags and properties
   - Visibility rules

2. **Parser & Syntax System**
   - SYNTAX rule definitions
   - Location constraints (HELD, ON-GROUND, IN-ROOM, TAKE)
   - Object resolution priority
   - Two-phase search algorithm

3. **Game Flow & Execution**
   - Main game loop (MAIN-LOOP)
   - PERFORM dispatch chain
   - Action handler return values
   - Turn processing

4. **Puzzles & Game Mechanics**
   - All major puzzles documented
   - Combat system details
   - Death conditions
   - Win conditions
   - Treasure scoring

5. **World Structure**
   - Complete room map (6 areas, 110 rooms)
   - Exit types and door mechanics
   - Conditional passages
   - Map divisions

6. **Container System**
   - CONTBIT flag mechanics
   - OPENBIT, TRANSBIT, SURFACEBIT
   - Capacity system
   - Visibility logic

---

## 7. Proposed Article Index: "How Zork Was Programmed"

Based on the existing documentation, here's a suggested structure for an article explaining Zork's programming:

### **Part I: Architecture Foundations**

1. **Introduction to ZIL (Zork Implementation Language)**
   - LISP/MDL heritage
   - Declarative vs imperative programming
   - Angle-bracket syntax
   - Variables: `,GLOBAL` vs `.LOCAL`

2. **The Object System**
   - Everything is an OBJECT
   - Object properties (SYNONYM, DESC, FLAGS, ACTION)
   - Object hierarchy and location tree
   - Flag system (TAKEBIT, ACTORBIT, DOORBIT, etc.)

3. **Object Visibility & Scoping**
   - Regular objects (single location)
   - GLOBAL-OBJECTS (visible everywhere)
   - LOCAL-GLOBALS (visible in specific rooms)
   - How the parser finds objects

### **Part II: The Parser Engine**

4. **Natural Language Parsing**
   - Tokenization (P-LEXV)
   - Lexical analysis
   - Word classification (verbs, nouns, adjectives, prepositions)
   - Pronoun handling (IT, THEM)

5. **The SYNTAX System**
   - Declarative grammar definitions
   - Object specifications
   - Location constraints (HELD, ON-GROUND, IN-ROOM, TAKE)
   - Flag requirements (FIND TAKEBIT)
   - Auto-take mechanism

6. **Object Resolution Priority**
   - Two-phase search algorithm
   - Inventory-first ordering
   - Room contents and globals
   - Open container searching

### **Part III: Action Dispatch & Execution**

7. **The PERFORM Dispatch Chain**
   - WINNER's action handler
   - Room's M-BEG action
   - PREACTIONS table
   - PRSI (indirect object) action
   - PRSO container action
   - PRSO (direct object) action
   - Default action handlers

8. **Verb Implementation**
   - V-TAKE, V-DROP, V-EXAMINE
   - Pre-actions (PRE-TAKE, etc.)
   - Action return values (M-HANDLED, M-NOT-HANDLED, M-FATAL)
   - Error handling

9. **Custom Action Routines**
   - Object-specific behaviors
   - Room-specific behaviors
   - Special interactions

### **Part IV: World Building**

10. **Room System**
    - Room properties (DESC, LDESC)
    - Directional exits
    - Conditional exits (CEXIT)
    - Function-based exits (FEXIT)
    - Door exits (DEXIT)

11. **Container & Door Mechanics**
    - CONTBIT container system
    - OPENBIT, TRANSBIT, SURFACEBIT
    - Capacity management
    - DOORBIT door system
    - Lock mechanisms

12. **Global State Management**
    - Game state flags (TROLL-FLAG, MAGIC-FLAG, etc.)
    - Puzzle state tracking
    - Score and death counters

### **Part V: Game Mechanics**

13. **Combat System**
    - Strength calculations
    - Villain table
    - Weapon modifiers
    - Combat result types
    - Injury and healing

14. **Death & Resurrection**
    - JIGS-UP handler
    - Three-strike system
    - Spirit world mechanics
    - PRAY resurrection

15. **Treasure & Scoring**
    - TVALUE property
    - Trophy case mechanism
    - Win condition (350 points)
    - Final sequence

16. **Puzzle Design Patterns**
    - Lock and key puzzles
    - Light/darkness management
    - Water puzzles
    - NPC interaction
    - Environmental puzzles
    - Timing puzzles

### **Part VI: Advanced Topics**

17. **Interrupt System (Daemons & Fuses)**
    - CLOCKER routine
    - Lamp battery countdown
    - Thief random appearances
    - Timed events

18. **Special Cases & Edge Cases**
    - Pseudo-objects
    - Special global objects (IT, ME, GROUND)
    - OOPS typo correction
    - Multiple object handling (ALL)

19. **Memory & Performance Optimization**
    - Why global objects save memory
    - Parser efficiency
    - Object tree navigation
    - Code reuse patterns

### **Part VII: Case Studies**

20. **Complete Walkthrough: "TAKE LAMP"**
    - Parse → Syntax match → Object resolution → PERFORM → Action

21. **Complex Puzzle Analysis: Rainbow Bridge**
    - SCEPTRE object
    - WAVE verb
    - RAINBOW-FLAG state
    - POT-OF-GOLD reveal

22. **NPC Behavior: The Thief**
    - Random appearance (interrupt)
    - Theft mechanics
    - Combat behavior
    - Treasure recovery

---

## 8. Gaps & Missing Information

### What's NOT Covered (or Needs More Detail):

#### 🔴 **Critical Gaps:**

1. **Interrupt System (CLOCKER/Daemons/Fuses)**
   - How timed events work
   - Daemon scheduling
   - Fuse countdown mechanics
   - Examples: lamp battery, thief appearances, cyclops countdown

2. **Module Loading & Initialization**
   - zork1.zil include order
   - Why module order matters
   - Initialization sequence
   - Bootstrap process

3. **Save/Restore System**
   - How game state is serialized
   - What gets saved
   - Restore mechanics

4. **Error Handling & Debugging**
   - Parser error messages
   - Invalid command handling
   - Debug commands
   - Testing infrastructure

5. **Vocabulary Tables**
   - How words are defined
   - Synonym tables
   - Adjective tables
   - Direction words

#### 🟡 **Medium Gaps:**

6. **Pre-actions System**
   - PRE-TAKE, PRE-DROP, etc.
   - When pre-actions run
   - How they block main actions
   - Examples

7. **PREACTIONS Table**
   - Global action interceptors
   - Priority and ordering
   - Use cases

8. **Pseudo-Objects**
   - How they work
   - Room-specific pseudo-objects
   - Examples: NAILS in living room

9. **OOPS Typo Correction**
   - How OOPS works
   - Word history tracking
   - Limitations

10. **Direction Handling**
    - How NORTH, SOUTH, etc. work
    - Difference from regular verbs
    - Walking mechanics

#### 🟢 **Minor Gaps:**

11. **Multi-Object Commands**
    - How "ALL" works
    - Object filtering
    - Iteration mechanics

12. **Adjective Handling**
    - How adjectives disambiguate
    - "TAKE RED BOOK" vs "TAKE BLUE BOOK"

13. **Memory Constraints**
    - 1980 memory limitations
    - Why code was optimized
    - Object count limits

14. **Development Tools**
    - Original compiler
    - ZILF modern compiler
    - Differences between them

15. **Z-Machine Target**
    - What is Z-machine
    - How ZIL compiles to Z-code
    - Bytecode format

16. **Historical Context**
    - Why LISP was chosen
    - Development team
    - Timeline of development

---

## 9. Recommended Documentation Additions

To complete the "How Zork Was Programmed" guide, these topics should be added:

### **Priority 1 (Critical):**
- **Interrupt System Guide** - Daemons, fuses, CLOCKER, timed events
- **Module System** - File inclusion order, initialization, dependencies
- **Vocabulary & Lexical Tables** - How words are defined and looked up

### **Priority 2 (Important):**
- **Pre-actions Deep Dive** - PRE-TAKE, PREACTIONS table, blocking mechanics
- **Direction & Movement** - Walking, direction verbs, exit processing
- **Pseudo-Objects Guide** - Room-specific pseudo-objects

### **Priority 3 (Nice to Have):**
- **Save/Restore Mechanics**
- **OOPS System**
- **Multi-Object Handling**
- **Historical Context & Development**
- **Z-Machine Compilation Target**

### **Priority 4 (Reference):**
- **Complete Verb Reference** - All 267 verbs documented
- **Complete Object Catalog** - All 200+ objects with properties
- **Complete Flag Reference** - All 24+ flags explained
- **Complete Room Map** - Visual diagram of all 110 rooms

---

## Summary

The existing documentation provides **excellent coverage** of:
- Object system architecture
- Parser and syntax rules
- PERFORM dispatch
- Puzzles and game mechanics
- World structure

The main **gaps** are:
- Interrupt/daemon system
- Module loading
- Vocabulary tables
- Pre-actions in detail
- Historical/development context

Adding these topics would create a **comprehensive guide** to understanding how this classic text adventure game was programmed, demonstrating timeless design patterns still relevant today!
