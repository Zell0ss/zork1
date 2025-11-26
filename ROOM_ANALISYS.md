# ZORK I MAP ANALYSIS


## MAP DIVISIONS (6 maps, ~15-20 rooms each):

   - MAP 1: ABOVE GROUND - HOUSE & FOREST (20 rooms)
   - MAP 2: CELLAR, GALLERY, MAZE, & CYCLOPS AREA (28 rooms)
   - MAP 3: ROUND ROOM AREA & TEMPLES (17 rooms)
   - MAP 4: RESERVOIR & DAM AREA (16 rooms)
   - MAP 5: RIVER AREA (17 rooms)
   - MAP 6: COAL MINE AREA (12 rooms)

  
### MAP 1: ABOVE GROUND - HOUSE & FOREST AREA (20 rooms)
 
   - WEST-OF-HOUSE
     >NORTH -> NORTH-OF-HOUSE
     >SOUTH -> SOUTH-OF-HOUSE
     >NE -> NORTH-OF-HOUSE
     >SE -> SOUTH-OF-HOUSE
     >WEST -> FOREST-1
     >SW -> STONE-BARROW (conditional: IF WON-FLAG)
     >IN -> STONE-BARROW (conditional: IF WON-FLAG)

   - STONE-BARROW
     >NE -> WEST-OF-HOUSE

   - NORTH-OF-HOUSE
     >SW -> WEST-OF-HOUSE
     >SE -> EAST-OF-HOUSE
     >WEST -> WEST-OF-HOUSE
     >EAST -> EAST-OF-HOUSE
     >NORTH -> PATH

   - SOUTH-OF-HOUSE
     >WEST -> WEST-OF-HOUSE
     >EAST -> EAST-OF-HOUSE
     >NE -> EAST-OF-HOUSE
     >NW -> WEST-OF-HOUSE
     >SOUTH -> FOREST-3

   - EAST-OF-HOUSE
     >NORTH -> NORTH-OF-HOUSE
     >SOUTH -> SOUTH-OF-HOUSE
     >SW -> SOUTH-OF-HOUSE
     >NW -> NORTH-OF-HOUSE
     >EAST -> CLEARING
     >WEST -> KITCHEN (conditional: IF KITCHEN-WINDOW IS OPEN)
     >IN -> KITCHEN (conditional: IF KITCHEN-WINDOW IS OPEN)

   - FOREST-1
     >NORTH -> GRATING-CLEARING
     >EAST -> PATH
     >SOUTH -> FOREST-3

   - FOREST-2
     >EAST -> MOUNTAINS
     >SOUTH -> CLEARING
     >WEST -> PATH

   - MOUNTAINS
     >NORTH -> FOREST-2
     >SOUTH -> FOREST-2
     >WEST -> FOREST-2

   - FOREST-3
     >NORTH -> CLEARING
     >WEST -> FOREST-1
     >NW -> SOUTH-OF-HOUSE

   - PATH
     >UP -> UP-A-TREE
     >NORTH -> GRATING-CLEARING
     >EAST -> FOREST-2
     >SOUTH -> NORTH-OF-HOUSE
     >WEST -> FOREST-1

   - UP-A-TREE
     >DOWN -> PATH

   - GRATING-CLEARING
     >EAST -> FOREST-2
     >WEST -> FOREST-1
     >SOUTH -> PATH
     >DOWN -> GRATING-ROOM (conditional via PER GRATING-EXIT)

   - CLEARING
     >EAST -> CANYON-VIEW
     >NORTH -> FOREST-2
     >SOUTH -> FOREST-3
     >WEST -> EAST-OF-HOUSE

   - KITCHEN
     >EAST -> EAST-OF-HOUSE (conditional: IF KITCHEN-WINDOW IS OPEN)
     >WEST -> LIVING-ROOM
     >OUT -> EAST-OF-HOUSE (conditional: IF KITCHEN-WINDOW IS OPEN)
     >UP -> ATTIC

   - ATTIC
     >DOWN -> KITCHEN

   - LIVING-ROOM
     >EAST -> KITCHEN
     >WEST -> STRANGE-PASSAGE (conditional: IF MAGIC-FLAG)
     >DOWN -> CELLAR (conditional via PER TRAP-DOOR-EXIT)

   - CANYON-VIEW
     >EAST -> CLIFF-MIDDLE
     >DOWN -> CLIFF-MIDDLE
     >NW -> CLEARING
     >WEST -> FOREST-3

   - CLIFF-MIDDLE
     >UP -> CANYON-VIEW
     >DOWN -> CANYON-BOTTOM

   - CANYON-BOTTOM
     >UP -> CLIFF-MIDDLE
     >NORTH -> END-OF-RAINBOW

   - END-OF-RAINBOW
     >UP -> ON-RAINBOW (conditional: IF RAINBOW-FLAG)
     >NE -> ON-RAINBOW (conditional: IF RAINBOW-FLAG)
     >EAST -> ON-RAINBOW (conditional: IF RAINBOW-FLAG)
     >SW -> CANYON-BOTTOM

### MAP 2: CELLAR, GALLERY, MAZE, & CYCLOPS AREA (28 rooms)

   - CELLAR
     >NORTH -> TROLL-ROOM
     >SOUTH -> EAST-OF-CHASM
     >UP -> LIVING-ROOM (conditional: IF TRAP-DOOR IS OPEN)

   - TROLL-ROOM
     >SOUTH -> CELLAR
     >EAST -> EW-PASSAGE (conditional: IF TROLL-FLAG)
     >WEST -> MAZE-1 (conditional: IF TROLL-FLAG)

   - EAST-OF-CHASM
     >NORTH -> CELLAR
     >EAST -> GALLERY

   - GALLERY
     >WEST -> EAST-OF-CHASM
     >NORTH -> STUDIO

   - STUDIO
     >SOUTH -> GALLERY
     >UP -> KITCHEN (conditional via PER UP-CHIMNEY-FUNCTION)

   - MAZE-1
     >EAST -> TROLL-ROOM
     >NORTH -> MAZE-1
     >SOUTH -> MAZE-2
     >WEST -> MAZE-4

   - MAZE-2
     >SOUTH -> MAZE-1
     >DOWN -> MAZE-4 (conditional via PER MAZE-DIODES)
     >EAST -> MAZE-3

   - MAZE-3
     >WEST -> MAZE-2
     >NORTH -> MAZE-4
     >UP -> MAZE-5

   - MAZE-4
     >WEST -> MAZE-3
     >NORTH -> MAZE-1
     >EAST -> DEAD-END-1

   - DEAD-END-1
     >SOUTH -> MAZE-4

   - MAZE-5
     >EAST -> DEAD-END-2
     >NORTH -> MAZE-3
     >SW -> MAZE-6

   - DEAD-END-2
     >WEST -> MAZE-5

   - MAZE-6
     >DOWN -> MAZE-5
     >EAST -> MAZE-7
     >WEST -> MAZE-6
     >UP -> MAZE-9

   - MAZE-7
     >UP -> MAZE-14
     >WEST -> MAZE-6
     >DOWN -> DEAD-END-1 (conditional via PER MAZE-DIODES)
     >EAST -> MAZE-8
     >SOUTH -> MAZE-15

   - MAZE-8
     >NE -> MAZE-7
     >WEST -> MAZE-8
     >SE -> DEAD-END-3

   - DEAD-END-3
     >NORTH -> MAZE-8

   - MAZE-9
     >NORTH -> MAZE-6
     >DOWN -> MAZE-11 (conditional via PER MAZE-DIODES)
     >EAST -> MAZE-10
     >SOUTH -> MAZE-13
     >WEST -> MAZE-12
     >NW -> MAZE-9

   - MAZE-10
     >EAST -> MAZE-9
     >WEST -> MAZE-13
     >UP -> MAZE-11

   - MAZE-11
     >NE -> GRATING-ROOM
     >DOWN -> MAZE-10
     >NW -> MAZE-13
     >SW -> MAZE-12

   - GRATING-ROOM
     >SW -> MAZE-11
     >UP -> GRATING-CLEARING (conditional: IF GRATE IS OPEN)

   - MAZE-12
     >DOWN -> MAZE-5 (conditional via PER MAZE-DIODES)
     >SW -> MAZE-11
     >EAST -> MAZE-13
     >UP -> MAZE-9
     >NORTH -> DEAD-END-4

   - DEAD-END-4
     >SOUTH -> MAZE-12

   - MAZE-13
     >EAST -> MAZE-9
     >DOWN -> MAZE-12
     >SOUTH -> MAZE-10
     >WEST -> MAZE-11

   - MAZE-14
     >WEST -> MAZE-15
     >NW -> MAZE-14
     >NE -> MAZE-7
     >SOUTH -> MAZE-7

   - MAZE-15
     >WEST -> MAZE-14
     >SOUTH -> MAZE-7
     >SE -> CYCLOPS-ROOM

   - CYCLOPS-ROOM
     >NW -> MAZE-15
     >EAST -> STRANGE-PASSAGE (conditional: IF MAGIC-FLAG)
     >UP -> TREASURE-ROOM (conditional: IF CYCLOPS-FLAG)

   - STRANGE-PASSAGE
     >WEST -> CYCLOPS-ROOM
     >IN -> CYCLOPS-ROOM
     >EAST -> LIVING-ROOM

   - TREASURE-ROOM
     >DOWN -> CYCLOPS-ROOM

### MAP 3: ROUND ROOM AREA, TEMPLES, & HADES (17 rooms)

   - EW-PASSAGE
     >EAST -> ROUND-ROOM
     >WEST -> TROLL-ROOM
     >DOWN -> CHASM-ROOM
     >NORTH -> CHASM-ROOM

   - ROUND-ROOM
     >EAST -> LOUD-ROOM
     >WEST -> EW-PASSAGE
     >NORTH -> NS-PASSAGE
     >SOUTH -> NARROW-PASSAGE
     >SE -> ENGRAVINGS-CAVE

   - LOUD-ROOM
     >EAST -> DAMP-CAVE
     >WEST -> ROUND-ROOM
     >UP -> DEEP-CANYON

   - DAMP-CAVE
     >WEST -> LOUD-ROOM
     >EAST -> WHITE-CLIFFS-NORTH

   - NS-PASSAGE
     >NORTH -> CHASM-ROOM
     >NE -> DEEP-CANYON
     >SOUTH -> ROUND-ROOM

   - CHASM-ROOM
     >NE -> RESERVOIR-SOUTH
     >SW -> EW-PASSAGE
     >UP -> EW-PASSAGE
     >SOUTH -> NS-PASSAGE

   - DEEP-CANYON
     >NW -> RESERVOIR-SOUTH
     >EAST -> DAM-ROOM
     >SW -> NS-PASSAGE
     >DOWN -> LOUD-ROOM

   - ENGRAVINGS-CAVE
     >NW -> ROUND-ROOM
     >EAST -> DOME-ROOM

   - DOME-ROOM
     >WEST -> ENGRAVINGS-CAVE
     >DOWN -> TORCH-ROOM (conditional: IF DOME-FLAG)

   - TORCH-ROOM
     >SOUTH -> NORTH-TEMPLE
     >DOWN -> NORTH-TEMPLE

   - NORTH-TEMPLE
     >DOWN -> EGYPT-ROOM
     >EAST -> EGYPT-ROOM
     >NORTH -> TORCH-ROOM
     >OUT -> TORCH-ROOM
     >UP -> TORCH-ROOM
     >SOUTH -> SOUTH-TEMPLE

   - EGYPT-ROOM
     >WEST -> NORTH-TEMPLE
     >UP -> NORTH-TEMPLE

   - SOUTH-TEMPLE
     >NORTH -> NORTH-TEMPLE
     >DOWN -> TINY-CAVE (conditional: IF COFFIN-CURE)

   - ENTRANCE-TO-HADES
     >UP -> TINY-CAVE
     >IN -> LAND-OF-LIVING-DEAD (conditional: IF LLD-FLAG)
     >SOUTH -> LAND-OF-LIVING-DEAD (conditional: IF LLD-FLAG)

   - LAND-OF-LIVING-DEAD
     >OUT -> ENTRANCE-TO-HADES
     >NORTH -> ENTRANCE-TO-HADES

   - MIRROR-ROOM-1
     >NORTH -> COLD-PASSAGE
     >WEST -> TWISTING-PASSAGE
     >EAST -> SMALL-CAVE

   - MIRROR-ROOM-2
     >WEST -> WINDING-PASSAGE
     >NORTH -> NARROW-PASSAGE
     >EAST -> TINY-CAVE

### MAP 4: RESERVOIR & DAM AREA (16 rooms)

   - RESERVOIR-SOUTH
     >SE -> DEEP-CANYON
     >SW -> CHASM-ROOM
     >EAST -> DAM-ROOM
     >WEST -> STREAM-VIEW
     >NORTH -> RESERVOIR (conditional: IF LOW-TIDE)

   - RESERVOIR
     >NORTH -> RESERVOIR-NORTH
     >SOUTH -> RESERVOIR-SOUTH
     >UP -> IN-STREAM
     >WEST -> IN-STREAM

   - RESERVOIR-NORTH
     >NORTH -> ATLANTIS-ROOM
     >SOUTH -> RESERVOIR (conditional: IF LOW-TIDE)

   - STREAM-VIEW
     >EAST -> RESERVOIR-SOUTH

   - IN-STREAM
     >LAND -> STREAM-VIEW
     >DOWN -> RESERVOIR
     >EAST -> RESERVOIR

   - DAM-ROOM
     >SOUTH -> DEEP-CANYON
     >DOWN -> DAM-BASE
     >EAST -> DAM-BASE
     >NORTH -> DAM-LOBBY
     >WEST -> RESERVOIR-SOUTH

   - DAM-LOBBY
     >SOUTH -> DAM-ROOM
     >NORTH -> MAINTENANCE-ROOM
     >EAST -> MAINTENANCE-ROOM

   - MAINTENANCE-ROOM
     >SOUTH -> DAM-LOBBY
     >WEST -> DAM-LOBBY

   - DAM-BASE
     >NORTH -> DAM-ROOM
     >UP -> DAM-ROOM

   - SMALL-CAVE
     >NORTH -> MIRROR-ROOM-1
     >DOWN -> ATLANTIS-ROOM
     >SOUTH -> ATLANTIS-ROOM
     >WEST -> TWISTING-PASSAGE

   - TINY-CAVE
     >NORTH -> MIRROR-ROOM-2
     >WEST -> WINDING-PASSAGE
     >DOWN -> ENTRANCE-TO-HADES

   - COLD-PASSAGE
     >SOUTH -> MIRROR-ROOM-1
     >WEST -> SLIDE-ROOM

   - NARROW-PASSAGE
     >NORTH -> ROUND-ROOM
     >SOUTH -> MIRROR-ROOM-2

   - WINDING-PASSAGE
     >NORTH -> MIRROR-ROOM-2
     >EAST -> TINY-CAVE

   - TWISTING-PASSAGE
     >NORTH -> MIRROR-ROOM-1
     >EAST -> SMALL-CAVE

   - ATLANTIS-ROOM
     >UP -> SMALL-CAVE
     >SOUTH -> RESERVOIR-NORTH

###MAP 5: RIVER AREA (17 rooms)

   - RIVER-1
     >WEST -> DAM-BASE
     >LAND -> DAM-BASE
     >DOWN -> RIVER-2

   - RIVER-2
     >DOWN -> RIVER-3

   - RIVER-3
     >DOWN -> RIVER-4
     >LAND -> WHITE-CLIFFS-NORTH
     >WEST -> WHITE-CLIFFS-NORTH

   - WHITE-CLIFFS-NORTH
     >SOUTH -> WHITE-CLIFFS-SOUTH (conditional: IF DEFLATE)
     >WEST -> DAMP-CAVE (conditional: IF DEFLATE)

   - WHITE-CLIFFS-SOUTH
     >NORTH -> WHITE-CLIFFS-NORTH (conditional: IF DEFLATE)

   - RIVER-4
     >DOWN -> RIVER-5
     >WEST -> WHITE-CLIFFS-SOUTH
     >EAST -> SANDY-BEACH

   - RIVER-5
     >EAST -> SHORE
     >LAND -> SHORE

   - SHORE
     >NORTH -> SANDY-BEACH
     >SOUTH -> ARAGAIN-FALLS

   - SANDY-BEACH
     >NE -> SANDY-CAVE
     >SOUTH -> SHORE

   - SANDY-CAVE
     >SW -> SANDY-BEACH

   - ARAGAIN-FALLS
     >WEST -> ON-RAINBOW (conditional: IF RAINBOW-FLAG)
     >NORTH -> SHORE
     >UP -> ON-RAINBOW (conditional: IF RAINBOW-FLAG)

   - ON-RAINBOW
     >WEST -> END-OF-RAINBOW
     >EAST -> ARAGAIN-FALLS

### MAP 6: COAL MINE AREA (12 rooms)

   - SLIDE-ROOM
     >EAST -> COLD-PASSAGE
     >NORTH -> MINE-ENTRANCE
     >DOWN -> CELLAR

   - MINE-ENTRANCE
     >SOUTH -> SLIDE-ROOM
     >IN -> SQUEEKY-ROOM
     >WEST -> SQUEEKY-ROOM

   - SQUEEKY-ROOM
     >NORTH -> BAT-ROOM
     >EAST -> MINE-ENTRANCE

   - BAT-ROOM
     >SOUTH -> SQUEEKY-ROOM
     >EAST -> SHAFT-ROOM

   - SHAFT-ROOM
     >WEST -> BAT-ROOM
     >NORTH -> SMELLY-ROOM

   - SMELLY-ROOM
     >DOWN -> GAS-ROOM
     >SOUTH -> SHAFT-ROOM

   - GAS-ROOM
     >UP -> SMELLY-ROOM
     >EAST -> MINE-1

   - MINE-1
     >NORTH -> GAS-ROOM
     >EAST -> MINE-1
     >NE -> MINE-2

   - MINE-2
     >NORTH -> MINE-2
     >SOUTH -> MINE-1
     >SE -> MINE-3

   - MINE-3
     >SOUTH -> MINE-3
     >SW -> MINE-4
     >EAST -> MINE-2

   - MINE-4
     >NORTH -> MINE-3
     >WEST -> MINE-4
     >DOWN -> LADDER-TOP

   - LADDER-TOP
     >DOWN -> LADDER-BOTTOM
     >UP -> MINE-4

   - LADDER-BOTTOM
     >SOUTH -> DEAD-END-5
     >WEST -> TIMBER-ROOM
     >UP -> LADDER-TOP

   - DEAD-END-5
     >NORTH -> LADDER-BOTTOM

   - TIMBER-ROOM
     >EAST -> LADDER-BOTTOM
     >WEST -> LOWER-SHAFT (conditional: IF EMPTY-HANDED)

   - LOWER-SHAFT
     >SOUTH -> MACHINE-ROOM
     >OUT -> TIMBER-ROOM (conditional: IF EMPTY-HANDED)
     >EAST -> TIMBER-ROOM (conditional: IF EMPTY-HANDED)

   - MACHINE-ROOM
     >NORTH -> LOWER-SHAFT

---

## How Doors Work in Zork

Doors in Zork use the **DOORBIT flag** combined with room exit definitions and action handlers. The system provides flexible mechanics for locked doors, one-way passages, and conditional access.

### Exit Types

From [gverbs.zil:2001-2013](gverbs.zil#L2001-L2013):

```python
# Exit type constants
REXIT = 0   # Regular exit - just go to the room
UEXIT = 1   # Unconditional exit with message
NEXIT = 2   # No exit with message
FEXIT = 3   # Function-based exit (calls a routine)
CEXIT = 4   # Conditional exit (check flag/condition)
DEXIT = 5   # Door exit (check if door object is open)
```

### Door System Components

#### 1. Door Object with DOORBIT

Door objects are placed in rooms and have the `DOORBIT` flag:

```python
class TRAP_DOOR(Object):
    location = LIVING_ROOM
    synonyms = ["DOOR", "TRAPDOOR", "TRAP-DOOR", "COVER"]
    adjectives = ["TRAP", "DUSTY"]
    description = "trap door"
    flags = [DOORBIT, NDESCBIT, INVISIBLE]
    action = trap_door_fcn  # Custom action handler
```

#### 2. Room Exit Definitions

Rooms reference doors using different exit types:

**CEXIT (Conditional)** - Check if door is open:
```python
class CELLAR(Room):
    description = "Cellar"
    exits = {
        UP: ConditionalExit(
            destination=LIVING_ROOM,
            condition=lambda: TRAP_DOOR.has_flag(OPENBIT)
        )
    }
```

This checks if the TRAP-DOOR has `OPENBIT` set.

**FEXIT (Function)** - Custom logic:
```python
class LIVING_ROOM(Room):
    description = "Living Room"
    exits = {
        DOWN: FunctionExit(
            function=trap_door_exit  # Custom function
        )
    }
```

This calls a function to determine what happens.

#### 3. Finding Where Doors Lead

The `OTHER-SIDE` routine at [gverbs.zil:2173-2181](gverbs.zil#L2173-L2181) finds the destination:

```python
def other_side(door_obj):
    """Find which direction a door leads in the current room"""
    for direction, exit_data in HERE.exits.items():
        # Check if this is a door exit
        if exit_data.exit_type == DEXIT:
            # Check if the door object matches
            if exit_data.door_object == door_obj:
                return direction  # Return the direction
    return None  # Door not found in this room
```

This searches the current room's exits for one that references the specific door object.

#### 4. Going Through Doors

From [gverbs.zil:1404-1409](gverbs.zil#L1404-L1409):

```python
def v_through():
    """Handle 'GO THROUGH <door>' command"""
    # Check if object is a door
    if PRSO.has_flag(DOORBIT):
        # Find which direction this door leads
        direction = other_side(PRSO)
        if direction:
            do_walk(direction)  # Walk in that direction
```

When you type "GO THROUGH DOOR", it finds which direction the door leads and walks that way.

#### 5. DEXIT Processing

From [gverbs.zil:1550-1555](gverbs.zil#L1550-L1555), when walking through a DEXIT:

```python
def process_exit(exit_data):
    """Process a door exit when player tries to walk through"""
    if exit_data.exit_type == DEXIT:
        door_obj = exit_data.door_object

        # Check if door is open
        if door_obj.has_flag(OPENBIT):
            goto(exit_data.destination)  # Door is open, go through
        else:
            # Door is closed, show message
            message = exit_data.closed_message
            print(message)
            return FATAL  # Stop processing turn
```

### Lock and Condition Examples

#### Example 1: Trap Door - Location-Based Behavior

From [1actions.zil:504-523](1actions.zil#L504-L523):

```python
def trap_door_fcn():
    """Handle trap door interactions based on location"""

    # From above (Living Room) - can open/close normally
    if current_verb in ["OPEN", "CLOSE"] and HERE == LIVING_ROOM:
        open_close(PRSO,
                  "The door reluctantly opens to reveal a rickety staircase descending into darkness.",
                  "The door swings shut and closes.")

    # From below (Cellar) - locked!
    elif HERE == CELLAR:
        if current_verb in ["OPEN", "UNLOCK"] and not TRAP_DOOR.has_flag(OPENBIT):
            print("The door is locked from above.")
```

**Behavior:**
- From LIVING-ROOM: Can open/close normally
- From CELLAR: **Locked** - can't open from below!

#### Example 2: Front Door - Permanently Locked

From [1actions.zil:2163-2172](1actions.zil#L2163-L2172):

```python
def front_door_fcn():
    """Handle front door - permanently locked"""
    if current_verb == "OPEN":
        print("The door cannot be opened.")
    elif current_verb == "BURN":
        print("You cannot burn this door.")
```

**Behavior:** Cannot be opened at all - boarded shut!

#### Example 3: Barrow Door - Always Open

From [1dungeon.zil:427-433](1dungeon.zil#L427-L433):

```python
class BARROW_DOOR(Object):
    flags = [DOORBIT, NDESCBIT, OPENBIT]  # Already open
    action = barrow_door_fcn

def barrow_door_fcn():
    """Handle barrow door - always open, too heavy to move"""
    if current_verb in ["OPEN", "CLOSE"]:
        print("The door is too heavy.")
```

**Behavior:** Always open, can't be closed (too heavy).

#### Example 4: Conditional Exits - Flag-Based

From [1dungeon.zil:1455](1dungeon.zil#L1455):

```python
class LIVING_ROOM(Room):
    exits = {
        WEST: ConditionalExit(
            destination=STRANGE_PASSAGE,
            condition=lambda: MAGIC_FLAG,
            failure_message="The door is nailed shut."
        )
    }
```

Not a DOORBIT door, but shows conditional access based on game state.

### Door System Summary

| Component | Purpose | Example |
|-----------|---------|---------|
| **DOORBIT flag** | Mark object as a door | TRAP-DOOR, FRONT-DOOR |
| **OPENBIT flag** | Track if door is open | Set/cleared during gameplay |
| **ACTION routine** | Handle door interactions | TRAP-DOOR-FCN handles OPEN/CLOSE |
| **DEXIT in room** | Link direction to door | `(UP TO ROOM IF DOOR IS OPEN)` |
| **OTHER-SIDE** | Find where door leads | Searches room exits for matching door |
| **V-THROUGH** | "GO THROUGH DOOR" command | Finds direction and walks |

### Door Capabilities

Doors can have:
- ✅ **Simple locks**: Check OPENBIT before allowing passage
- ✅ **One-way locks**: Trap door locks from one side only
- ✅ **Permanent locks**: Front door can never open
- ✅ **Conditional access**: Require flags or items to open
- ✅ **Custom messages**: Different messages for each state

The system is flexible - door behavior is controlled by the ACTION routine, while navigation is handled by exit types.