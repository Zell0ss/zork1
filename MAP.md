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

### MAP 5: RIVER AREA (17 rooms)

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


## 9. World Maps - Room Connections

The following maps show the 110 rooms in Zork I and their directional connections. The game world has been divided into 6 geographic areas for clarity.

### Map 1: Above Ground - House & Forest Area

This map shows the surface world around the White House and surrounding forest.

```mermaid
%%{init: {'theme':'dark'}}%%
graph TB
    WEST-OF-HOUSE["WEST-OF-HOUSE"]
    NORTH-OF-HOUSE["NORTH-OF-HOUSE"]
    SOUTH-OF-HOUSE["SOUTH-OF-HOUSE"]
    EAST-OF-HOUSE["EAST-OF-HOUSE"]
    STONE-BARROW["STONE-BARROW"]
    FOREST-1["FOREST-1"]
    FOREST-2["FOREST-2"]
    FOREST-3["FOREST-3"]
    PATH["PATH"]
    MOUNTAINS["MOUNTAINS"]
    UP-A-TREE["UP-A-TREE"]
    GRATING-CLEARING["GRATING-CLEARING"]
    CLEARING["CLEARING"]
    CANYON-VIEW["CANYON-VIEW"]
    CLIFF-MIDDLE["CLIFF-MIDDLE"]
    CANYON-BOTTOM["CANYON-BOTTOM"]
    END-OF-RAINBOW["END-OF-RAINBOW"]
    KITCHEN["KITCHEN"]
    ATTIC["ATTIC"]
    LIVING-ROOM["LIVING-ROOM"]

    %% House perimeter
    WEST-OF-HOUSE -->|N| NORTH-OF-HOUSE
    WEST-OF-HOUSE -->|NE| NORTH-OF-HOUSE
    WEST-OF-HOUSE -->|S| SOUTH-OF-HOUSE
    WEST-OF-HOUSE -->|SE| SOUTH-OF-HOUSE

    NORTH-OF-HOUSE -->|E| EAST-OF-HOUSE
    NORTH-OF-HOUSE -->|SE| EAST-OF-HOUSE
    NORTH-OF-HOUSE -->|W| WEST-OF-HOUSE
    NORTH-OF-HOUSE -->|SW| WEST-OF-HOUSE
    NORTH-OF-HOUSE -->|N| PATH

    SOUTH-OF-HOUSE -->|E| EAST-OF-HOUSE
    SOUTH-OF-HOUSE -->|NE| EAST-OF-HOUSE
    SOUTH-OF-HOUSE -->|W| WEST-OF-HOUSE
    SOUTH-OF-HOUSE -->|NW| WEST-OF-HOUSE
    SOUTH-OF-HOUSE -->|S| FOREST-3

    EAST-OF-HOUSE -->|N| NORTH-OF-HOUSE
    EAST-OF-HOUSE -->|NW| NORTH-OF-HOUSE
    EAST-OF-HOUSE -->|S| SOUTH-OF-HOUSE
    EAST-OF-HOUSE -->|SW| SOUTH-OF-HOUSE
    EAST-OF-HOUSE -->|E| CLEARING

    STONE-BARROW -->|NE| WEST-OF-HOUSE

    %% Forest area
    FOREST-1 -->|N| GRATING-CLEARING
    FOREST-1 -->|E| PATH
    FOREST-1 -->|S| FOREST-3
    WEST-OF-HOUSE -->|W| FOREST-1

    FOREST-2 -->|W| PATH
    FOREST-2 -->|E| MOUNTAINS
    FOREST-2 -->|S| CLEARING

    FOREST-3 -->|N| CLEARING
    FOREST-3 -->|W| FOREST-1
    FOREST-3 -->|NW| SOUTH-OF-HOUSE

    PATH -->|N| GRATING-CLEARING
    PATH -->|E| FOREST-2
    PATH -->|W| FOREST-1
    PATH -->|S| NORTH-OF-HOUSE
    PATH -->|UP| UP-A-TREE

    MOUNTAINS -->|N| FOREST-2
    MOUNTAINS -->|S| FOREST-2
    MOUNTAINS -->|W| FOREST-2

    UP-A-TREE -->|DOWN| PATH

    GRATING-CLEARING -->|E| FOREST-2
    GRATING-CLEARING -->|W| FOREST-1
    GRATING-CLEARING -->|S| PATH

    CLEARING -->|N| FOREST-2
    CLEARING -->|S| FOREST-3
    CLEARING -->|W| EAST-OF-HOUSE
    CLEARING -->|E| CANYON-VIEW

    %% Canyon area
    CANYON-VIEW -->|NW| CLEARING
    CANYON-VIEW -->|W| FOREST-3
    CANYON-VIEW -->|E| CLIFF-MIDDLE
    CANYON-VIEW -->|DOWN| CLIFF-MIDDLE

    CLIFF-MIDDLE -->|UP| CANYON-VIEW
    CLIFF-MIDDLE -->|DOWN| CANYON-BOTTOM

    CANYON-BOTTOM -->|UP| CLIFF-MIDDLE
    CANYON-BOTTOM -->|N| END-OF-RAINBOW

    END-OF-RAINBOW -->|SW| CANYON-BOTTOM

    %% Inside house
    KITCHEN -->|W| LIVING-ROOM
    KITCHEN -->|UP| ATTIC

    ATTIC -->|DOWN| KITCHEN

    LIVING-ROOM -->|E| KITCHEN

    %% Connections to other maps
    GRATING-CLEARING -.->|DOWN> To Map 2| GRATING-ROOM
    LIVING-ROOM -.->|DOWN> To Map 2| CELLAR
    LIVING-ROOM -.->|W> To Map 2| STRANGE-PASSAGE

    style WEST-OF-HOUSE fill:#90EE90
    style LIVING-ROOM fill:#FFD700
    style KITCHEN fill:#FFD700
    style ATTIC fill:#FFD700
```

**Key locations:**
- **WEST-OF-HOUSE**: Starting location of the game
- **LIVING-ROOM**: Entrance to the house, connects to underground via cellar
- **GRATING-CLEARING**: Surface entrance to underground via grating

---

### Map 2: Cellar, Gallery, Maze & Cyclops Area

This map shows the maze complex and the areas accessible from the cellar.

```mermaid
graph TB
    CELLAR["CELLAR"]
    TROLL-ROOM["TROLL-ROOM"]
    EAST-OF-CHASM["EAST-OF-CHASM"]
    GALLERY["GALLERY"]
    STUDIO["STUDIO"]
    GRATING-ROOM["GRATING-ROOM"]
    MAZE-1["MAZE-1"]
    MAZE-2["MAZE-2"]
    MAZE-3["MAZE-3"]
    MAZE-4["MAZE-4"]
    MAZE-5["MAZE-5"]
    MAZE-6["MAZE-6"]
    MAZE-7["MAZE-7"]
    MAZE-8["MAZE-8"]
    MAZE-9["MAZE-9"]
    MAZE-10["MAZE-10"]
    MAZE-11["MAZE-11"]
    MAZE-12["MAZE-12"]
    MAZE-13["MAZE-13"]
    MAZE-14["MAZE-14"]
    MAZE-15["MAZE-15"]
    DEAD-END-1["DEAD-END-1"]
    DEAD-END-2["DEAD-END-2"]
    DEAD-END-3["DEAD-END-3"]
    DEAD-END-4["DEAD-END-4"]
    CYCLOPS-ROOM["CYCLOPS-ROOM"]
    STRANGE-PASSAGE["STRANGE-PASSAGE"]
    TREASURE-ROOM["TREASURE-ROOM"]

    %% Entry area
    CELLAR -->|N| TROLL-ROOM
    CELLAR -->|S| EAST-OF-CHASM

    TROLL-ROOM -->|S| CELLAR

    EAST-OF-CHASM -->|N| CELLAR
    EAST-OF-CHASM -->|E| GALLERY

    GALLERY -->|W| EAST-OF-CHASM
    GALLERY -->|N| STUDIO

    STUDIO -->|S| GALLERY

    %% Maze entrance
    TROLL-ROOM -->|W| MAZE-1

    MAZE-1 -->|E| TROLL-ROOM
    MAZE-1 -->|S| MAZE-2
    MAZE-1 -->|W| MAZE-4
    MAZE-1 -->|N| MAZE-1

    MAZE-2 -->|N| MAZE-1
    MAZE-2 -->|E| MAZE-3

    MAZE-3 -->|W| MAZE-2
    MAZE-3 -->|N| MAZE-4
    MAZE-3 -->|UP| MAZE-5

    MAZE-4 -->|S| MAZE-3
    MAZE-4 -->|N| MAZE-1
    MAZE-4 -->|E| DEAD-END-1

    DEAD-END-1 -->|S| MAZE-4

    MAZE-5 -->|DOWN| MAZE-3
    MAZE-5 -->|N| MAZE-3
    MAZE-5 -->|SW| MAZE-6
    MAZE-5 -->|E| DEAD-END-2

    DEAD-END-2 -->|W| MAZE-5

    MAZE-6 -->|UP| MAZE-5
    MAZE-6 -->|DOWN| MAZE-5
    MAZE-6 -->|E| MAZE-7
    MAZE-6 -->|W| MAZE-6

    MAZE-7 -->|W| MAZE-6
    MAZE-7 -->|E| MAZE-8
    MAZE-7 -->|UP| MAZE-14
    MAZE-7 -->|S| MAZE-15

    MAZE-8 -->|W| MAZE-8
    MAZE-8 -->|NE| MAZE-7
    MAZE-8 -->|SE| DEAD-END-3

    DEAD-END-3 -->|N| MAZE-8

    MAZE-9 -->|N| MAZE-6
    MAZE-9 -->|NW| MAZE-9
    MAZE-9 -->|E| MAZE-10
    MAZE-9 -->|W| MAZE-12
    MAZE-9 -->|S| MAZE-13

    MAZE-10 -->|W| MAZE-9
    MAZE-10 -->|E| MAZE-9
    MAZE-10 -->|S| MAZE-13
    MAZE-10 -->|UP| MAZE-11

    MAZE-11 -->|DOWN| MAZE-10
    MAZE-11 -->|NW| MAZE-13
    MAZE-11 -->|SW| MAZE-12
    MAZE-11 -->|NE| GRATING-ROOM

    MAZE-12 -->|NE| MAZE-11
    MAZE-12 -->|E| MAZE-13
    MAZE-12 -->|UP| MAZE-9
    MAZE-12 -->|N| DEAD-END-4

    DEAD-END-4 -->|S| MAZE-12

    MAZE-13 -->|W| MAZE-11
    MAZE-13 -->|E| MAZE-9
    MAZE-13 -->|N| MAZE-10
    MAZE-13 -->|DOWN| MAZE-12

    MAZE-14 -->|DOWN| MAZE-7
    MAZE-14 -->|NE| MAZE-7
    MAZE-14 -->|S| MAZE-7
    MAZE-14 -->|NW| MAZE-14
    MAZE-14 -->|W| MAZE-15

    MAZE-15 -->|E| MAZE-14
    MAZE-15 -->|S| MAZE-7
    MAZE-15 -->|SE| CYCLOPS-ROOM

    %% Maze exit
    GRATING-ROOM -->|SW| MAZE-11

    %% Cyclops area
    CYCLOPS-ROOM -->|NW| MAZE-15

    STRANGE-PASSAGE -->|W| CYCLOPS-ROOM
    STRANGE-PASSAGE -->|IN| CYCLOPS-ROOM

    TREASURE-ROOM -->|DOWN| CYCLOPS-ROOM

    %% Connections to other maps
    CELLAR -.->|UP> To Map 1| LIVING-ROOM
    GRATING-ROOM -.->|UP> To Map 1| GRATING-CLEARING
    STUDIO -.->|UP> To Map 1| KITCHEN
    STRANGE-PASSAGE -.->|E> To Map 1| LIVING-ROOM
    TROLL-ROOM -.->|E> To Map 3| EW-PASSAGE

    style CELLAR fill:#FFB6C1
    style TROLL-ROOM fill:#FF6347
    style CYCLOPS-ROOM fill:#FF6347
    style GRATING-ROOM fill:#90EE90
```

**Key locations:**
- **CELLAR**: Main entrance from Living Room
- **TROLL-ROOM**: Guarded by troll, gateway to maze
- **MAZE-1 to MAZE-15**: Confusing maze with self-loops and dead ends
- **CYCLOPS-ROOM**: Boss area with cyclops
- **GRATING-ROOM**: Exit from maze to surface

---

### Map 3: Round Room, Temples & Hades Area

This map shows the central hub (Round Room) and the temple complex.

```mermaid
graph TB
    ROUND-ROOM["ROUND-ROOM"]
    EW-PASSAGE["EW-PASSAGE"]
    NS-PASSAGE["NS-PASSAGE"]
    LOUD-ROOM["LOUD-ROOM"]
    DAMP-CAVE["DAMP-CAVE"]
    CHASM-ROOM["CHASM-ROOM"]
    DEEP-CANYON["DEEP-CANYON"]
    ENGRAVINGS-CAVE["ENGRAVINGS-CAVE"]
    DOME-ROOM["DOME-ROOM"]
    TORCH-ROOM["TORCH-ROOM"]
    NORTH-TEMPLE["NORTH-TEMPLE"]
    SOUTH-TEMPLE["SOUTH-TEMPLE"]
    EGYPT-ROOM["EGYPT-ROOM"]
    ENTRANCE-TO-HADES["ENTRANCE-TO-HADES"]
    LAND-OF-LIVING-DEAD["LAND-OF-LIVING-DEAD"]
    NARROW-PASSAGE["NARROW-PASSAGE"]
    MIRROR-ROOM-2["MIRROR-ROOM-2"]

    %% Central hub - Round Room
    ROUND-ROOM -->|N| NS-PASSAGE
    ROUND-ROOM -->|S| NARROW-PASSAGE
    ROUND-ROOM -->|E| LOUD-ROOM
    ROUND-ROOM -->|W| EW-PASSAGE
    ROUND-ROOM -->|SE| ENGRAVINGS-CAVE

    EW-PASSAGE -->|E| ROUND-ROOM
    EW-PASSAGE -->|N| CHASM-ROOM
    EW-PASSAGE -->|DOWN| CHASM-ROOM

    NS-PASSAGE -->|S| ROUND-ROOM
    NS-PASSAGE -->|N| CHASM-ROOM
    NS-PASSAGE -->|NE| DEEP-CANYON

    LOUD-ROOM -->|W| ROUND-ROOM
    LOUD-ROOM -->|E| DAMP-CAVE
    LOUD-ROOM -->|UP| DEEP-CANYON

    DAMP-CAVE -->|W| LOUD-ROOM

    %% Canyon area
    CHASM-ROOM -->|S| NS-PASSAGE
    CHASM-ROOM -->|SW| EW-PASSAGE
    CHASM-ROOM -->|UP| EW-PASSAGE

    DEEP-CANYON -->|DOWN| LOUD-ROOM
    DEEP-CANYON -->|SW| NS-PASSAGE

    %% Engravings and Dome
    ENGRAVINGS-CAVE -->|NW| ROUND-ROOM
    ENGRAVINGS-CAVE -->|E| DOME-ROOM

    DOME-ROOM -->|W| ENGRAVINGS-CAVE

    %% Temple complex
    TORCH-ROOM -->|DOWN| NORTH-TEMPLE
    TORCH-ROOM -->|S| NORTH-TEMPLE

    NORTH-TEMPLE -->|UP| TORCH-ROOM
    NORTH-TEMPLE -->|N| TORCH-ROOM
    NORTH-TEMPLE -->|OUT| TORCH-ROOM
    NORTH-TEMPLE -->|S| SOUTH-TEMPLE
    NORTH-TEMPLE -->|DOWN| EGYPT-ROOM
    NORTH-TEMPLE -->|E| EGYPT-ROOM

    SOUTH-TEMPLE -->|N| NORTH-TEMPLE

    EGYPT-ROOM -->|UP| NORTH-TEMPLE
    EGYPT-ROOM -->|W| NORTH-TEMPLE

    %% Hades
    ENTRANCE-TO-HADES -->|UP| TINY-CAVE

    LAND-OF-LIVING-DEAD -->|N| ENTRANCE-TO-HADES
    LAND-OF-LIVING-DEAD -->|OUT| ENTRANCE-TO-HADES

    %% Mirror passages
    NARROW-PASSAGE -->|N| ROUND-ROOM
    NARROW-PASSAGE -->|S| MIRROR-ROOM-2

    MIRROR-ROOM-2 -->|N| NARROW-PASSAGE

    %% Connections to other maps
    EW-PASSAGE -.->|W> To Map 2| TROLL-ROOM
    CHASM-ROOM -.->|NE> To Map 4| RESERVOIR-SOUTH
    DEEP-CANYON -.->|E> To Map 4| DAM-ROOM
    DEEP-CANYON -.->|NW> To Map 4| RESERVOIR-SOUTH
    DAMP-CAVE -.->|E> To Map 5| WHITE-CLIFFS-NORTH
    MIRROR-ROOM-2 -.->|E> To Map 4| TINY-CAVE
    MIRROR-ROOM-2 -.->|W> To Map 4| WINDING-PASSAGE
    ENTRANCE-TO-HADES -.->|UP> To Map 4| TINY-CAVE

    style ROUND-ROOM fill:#FFD700
    style ENTRANCE-TO-HADES fill:#8B0000
    style LAND-OF-LIVING-DEAD fill:#8B0000
```

**Key locations:**
- **ROUND-ROOM**: Central hub with exits in all cardinal directions
- **TORCH-ROOM**: Temple area entrance
- **ENTRANCE-TO-HADES**: Gateway to the underworld
- **LAND-OF-LIVING-DEAD**: Deepest point in Hades

---

### Map 4: Reservoir & Dam Area

This map shows the water management system and mirror room passages.

```mermaid
graph TB
    RESERVOIR-SOUTH["RESERVOIR-SOUTH"]
    RESERVOIR["RESERVOIR"]
    RESERVOIR-NORTH["RESERVOIR-NORTH"]
    STREAM-VIEW["STREAM-VIEW"]
    IN-STREAM["IN-STREAM"]
    DAM-ROOM["DAM-ROOM"]
    DAM-LOBBY["DAM-LOBBY"]
    DAM-BASE["DAM-BASE"]
    MAINTENANCE-ROOM["MAINTENANCE-ROOM"]
    ATLANTIS-ROOM["ATLANTIS-ROOM"]
    SMALL-CAVE["SMALL-CAVE"]
    TINY-CAVE["TINY-CAVE"]
    MIRROR-ROOM-1["MIRROR-ROOM-1"]
    COLD-PASSAGE["COLD-PASSAGE"]
    TWISTING-PASSAGE["TWISTING-PASSAGE"]
    WINDING-PASSAGE["WINDING-PASSAGE"]

    %% Reservoir system
    RESERVOIR-SOUTH -->|W| STREAM-VIEW
    RESERVOIR-SOUTH -->|E| DAM-ROOM

    RESERVOIR -->|S| RESERVOIR-SOUTH
    RESERVOIR -->|N| RESERVOIR-NORTH
    RESERVOIR -->|W| IN-STREAM
    RESERVOIR -->|UP| IN-STREAM

    RESERVOIR-NORTH -->|N| ATLANTIS-ROOM

    STREAM-VIEW -->|E| RESERVOIR-SOUTH

    IN-STREAM -->|E| RESERVOIR
    IN-STREAM -->|DOWN| RESERVOIR
    IN-STREAM -->|LAND| STREAM-VIEW

    %% Dam system
    DAM-ROOM -->|W| RESERVOIR-SOUTH
    DAM-ROOM -->|N| DAM-LOBBY
    DAM-ROOM -->|DOWN| DAM-BASE
    DAM-ROOM -->|E| DAM-BASE

    DAM-LOBBY -->|S| DAM-ROOM
    DAM-LOBBY -->|N| MAINTENANCE-ROOM
    DAM-LOBBY -->|E| MAINTENANCE-ROOM

    MAINTENANCE-ROOM -->|S| DAM-LOBBY
    MAINTENANCE-ROOM -->|W| DAM-LOBBY

    DAM-BASE -->|N| DAM-ROOM
    DAM-BASE -->|UP| DAM-ROOM

    %% Atlantis and mirror passages
    ATLANTIS-ROOM -->|S| RESERVOIR-NORTH
    ATLANTIS-ROOM -->|UP| SMALL-CAVE

    SMALL-CAVE -->|DOWN| ATLANTIS-ROOM
    SMALL-CAVE -->|S| ATLANTIS-ROOM
    SMALL-CAVE -->|W| TWISTING-PASSAGE
    SMALL-CAVE -->|N| MIRROR-ROOM-1

    MIRROR-ROOM-1 -->|S| SMALL-CAVE
    MIRROR-ROOM-1 -->|N| COLD-PASSAGE
    MIRROR-ROOM-1 -->|E| SMALL-CAVE
    MIRROR-ROOM-1 -->|W| TWISTING-PASSAGE

    TWISTING-PASSAGE -->|E| SMALL-CAVE
    TWISTING-PASSAGE -->|N| MIRROR-ROOM-1

    COLD-PASSAGE -->|S| MIRROR-ROOM-1

    TINY-CAVE -->|N| MIRROR-ROOM-2
    TINY-CAVE -->|W| WINDING-PASSAGE

    WINDING-PASSAGE -->|E| TINY-CAVE
    WINDING-PASSAGE -->|N| MIRROR-ROOM-2

    %% Connections to other maps
    RESERVOIR-SOUTH -.->|SW To Map 3| CHASM-ROOM
    RESERVOIR-SOUTH -.->|SE To Map 3| DEEP-CANYON
    DAM-ROOM -.->|S To Map 3| DEEP-CANYON
    DAM-BASE -.->|W To Map 5| RIVER-1
    COLD-PASSAGE -.->|W To Map 6| SLIDE-ROOM
    TINY-CAVE -.->|DOWN To Map 3| ENTRANCE-TO-HADES
    MIRROR-ROOM-2 -.->|N To Map 3| NARROW-PASSAGE

    style ATLANTIS-ROOM fill:#00CED1
    style RESERVOIR fill:#4682B4
    style DAM-ROOM fill:#A9A9A9
```

**Key locations:**
- **RESERVOIR**: Main water storage area
- **DAM-ROOM**: Control area for the dam
- **ATLANTIS-ROOM**: Legendary sunken city (accessible when reservoir is empty)
- **MIRROR-ROOM-1**: Mirror passage connecting to Atlantis area

---

### Map 5: Frigid River & Shore Area

This map shows the river journey from the dam to the shore and waterfall.

```mermaid
graph TB
    RIVER-1["RIVER-1"]
    RIVER-2["RIVER-2"]
    RIVER-3["RIVER-3"]
    RIVER-4["RIVER-4"]
    RIVER-5["RIVER-5"]
    WHITE-CLIFFS-NORTH["WHITE-CLIFFS-NORTH"]
    WHITE-CLIFFS-SOUTH["WHITE-CLIFFS-SOUTH"]
    SANDY-BEACH["SANDY-BEACH"]
    SANDY-CAVE["SANDY-CAVE"]
    SHORE["SHORE"]
    ARAGAIN-FALLS["ARAGAIN-FALLS"]
    ON-RAINBOW["ON-RAINBOW"]

    %% River flow (westward)
    RIVER-1 -->|DOWN| RIVER-2
    RIVER-1 -->|LAND| DAM-BASE

    RIVER-2 -->|DOWN| RIVER-3

    RIVER-3 -->|DOWN| RIVER-4
    RIVER-3 -->|W| WHITE-CLIFFS-NORTH
    RIVER-3 -->|LAND| WHITE-CLIFFS-NORTH

    RIVER-4 -->|DOWN| RIVER-5
    RIVER-4 -->|E| SANDY-BEACH
    RIVER-4 -->|W| WHITE-CLIFFS-SOUTH

    RIVER-5 -->|E| SHORE
    RIVER-5 -->|LAND| SHORE

    %% Shore area
    SHORE -->|N| SANDY-BEACH
    SHORE -->|S| ARAGAIN-FALLS

    SANDY-BEACH -->|S| SHORE
    SANDY-BEACH -->|NE| SANDY-CAVE

    SANDY-CAVE -->|SW| SANDY-BEACH

    %% Waterfall and rainbow
    ARAGAIN-FALLS -->|N| SHORE

    ON-RAINBOW -->|E| ARAGAIN-FALLS
    ON-RAINBOW -->|W| END-OF-RAINBOW

    %% Connections to other maps
    RIVER-1 -.->|W To Map 4| DAM-BASE
    WHITE-CLIFFS-NORTH -.->|W To Map 3| DAMP-CAVE
    ON-RAINBOW -.->|W To Map 1| END-OF-RAINBOW
    ARAGAIN-FALLS -.->|W To Map 1| END-OF-RAINBOW

    style RIVER-1 fill:#87CEEB
    style RIVER-2 fill:#87CEEB
    style RIVER-3 fill:#87CEEB
    style RIVER-4 fill:#87CEEB
    style RIVER-5 fill:#87CEEB
    style SHORE fill:#F4A460
    style SANDY-BEACH fill:#F4A460
    style ON-RAINBOW fill:#FF69B4
```

**Key locations:**
- **RIVER-1 to RIVER-5**: The Frigid River flowing from dam to shore
- **SHORE**: Sandy beach at the end of the river
- **ARAGAIN-FALLS**: Spectacular waterfall
- **ON-RAINBOW**: Standing on the rainbow above the falls

---

### Map 6: Coal Mine Area

This map shows the coal mine complex with its tunnels, shafts, and machinery.

```mermaid
graph TB
    SLIDE-ROOM["SLIDE-ROOM"]
    MINE-ENTRANCE["MINE-ENTRANCE"]
    SQUEEKY-ROOM["SQUEEKY-ROOM"]
    BAT-ROOM["BAT-ROOM"]
    SHAFT-ROOM["SHAFT-ROOM"]
    SMELLY-ROOM["SMELLY-ROOM"]
    GAS-ROOM["GAS-ROOM"]
    MINE-1["MINE-1"]
    MINE-2["MINE-2"]
    MINE-3["MINE-3"]
    MINE-4["MINE-4"]
    LADDER-TOP["LADDER-TOP"]
    LADDER-BOTTOM["LADDER-BOTTOM"]
    TIMBER-ROOM["TIMBER-ROOM"]
    DEAD-END-5["DEAD-END-5"]
    LOWER-SHAFT["LOWER-SHAFT"]
    MACHINE-ROOM["MACHINE-ROOM"]

    %% Slide room entrance
    SLIDE-ROOM -->|N| MINE-ENTRANCE
    SLIDE-ROOM -->|E| COLD-PASSAGE

    MINE-ENTRANCE -->|S| SLIDE-ROOM
    MINE-ENTRANCE -->|W| SQUEEKY-ROOM
    MINE-ENTRANCE -->|IN| SQUEEKY-ROOM

    SQUEEKY-ROOM -->|E| MINE-ENTRANCE
    SQUEEKY-ROOM -->|N| BAT-ROOM

    %% Bat and shaft area
    BAT-ROOM -->|S| SQUEEKY-ROOM
    BAT-ROOM -->|E| SHAFT-ROOM

    SHAFT-ROOM -->|W| BAT-ROOM
    SHAFT-ROOM -->|N| SMELLY-ROOM

    SMELLY-ROOM -->|S| SHAFT-ROOM
    SMELLY-ROOM -->|DOWN| GAS-ROOM

    %% Gas room and mine tunnels
    GAS-ROOM -->|UP| SMELLY-ROOM
    GAS-ROOM -->|E| MINE-1

    MINE-1 -->|N| GAS-ROOM
    MINE-1 -->|NE| MINE-2
    MINE-1 -->|E| MINE-1

    MINE-2 -->|S| MINE-1
    MINE-2 -->|SE| MINE-3
    MINE-2 -->|N| MINE-2

    MINE-3 -->|E| MINE-2
    MINE-3 -->|SW| MINE-4
    MINE-3 -->|S| MINE-3

    MINE-4 -->|N| MINE-3
    MINE-4 -->|DOWN| LADDER-TOP
    MINE-4 -->|W| MINE-4

    %% Ladder and lower levels
    LADDER-TOP -->|UP| MINE-4
    LADDER-TOP -->|DOWN| LADDER-BOTTOM

    LADDER-BOTTOM -->|UP| LADDER-TOP
    LADDER-BOTTOM -->|W| TIMBER-ROOM
    LADDER-BOTTOM -->|S| DEAD-END-5

    DEAD-END-5 -->|N| LADDER-BOTTOM

    TIMBER-ROOM -->|E| LADDER-BOTTOM

    %% Machine room (isolated)
    LOWER-SHAFT -->|S| MACHINE-ROOM

    MACHINE-ROOM -->|N| LOWER-SHAFT

    %% Connections to other maps
    SLIDE-ROOM -.->|DOWN To Map 2| CELLAR
    SLIDE-ROOM -.->|E To Map 4| COLD-PASSAGE

    style SLIDE-ROOM fill:#90EE90
    style GAS-ROOM fill:#FFD700
    style MACHINE-ROOM fill:#A9A9A9
```

**Key locations:**
- **SLIDE-ROOM**: Main entrance to coal mine from cellar
- **MINE-ENTRANCE**: Entry point to the mine tunnels
- **GAS-ROOM**: Dangerous area with coal gas
- **MINE-1 to MINE-4**: Coal mine tunnels with self-loops
- **MACHINE-ROOM**: Bottom of the mine with machinery

---

## Map Summary

The Zork I world consists of **110 interconnected rooms** organized into 6 distinct geographic areas:

1. **Map 1** (20 rooms): Surface world with the White House, forest, and canyon
2. **Map 2** (28 rooms): Underground maze complex and gallery
3. **Map 3** (17 rooms): Central Round Room hub, temples, and Hades
4. **Map 4** (16 rooms): Reservoir system, dam, and Atlantis
5. **Map 5** (12 rooms): Frigid River flowing to shore and waterfall
6. **Map 6** (17 rooms): Coal mine tunnels and machinery

**Navigation notes:**
- Dotted lines (`.->`) indicate connections between maps
- Many rooms have conditional exits based on game state (flags, objects, etc.)
- The maze (Map 2) contains self-referencing exits that make navigation confusing
- Some mine tunnels (Map 6) also have self-loops for disorientation
