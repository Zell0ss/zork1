# Puzzle Mechanics, Combat, Death, and Win Conditions in Zork I

This document comprehensively covers all game mechanics in Zork I: combat system, death conditions, win conditions, and puzzle types.

---

## Table of Contents

1. [Combat System](#1-combat-system)
2. [Death Conditions](#2-death-conditions)
3. [Win Condition](#3-win-condition)
4. [Treasure List](#4-treasure-list)
5. [Puzzle Types](#5-puzzle-types)
   - [Lock and Key Puzzles](#a-lock-and-key-puzzles)
   - [Light/Darkness Puzzles](#b-lightdarkness-puzzles)
   - [Water Puzzles](#c-water-puzzles)
   - [NPC Interaction Puzzles](#d-npc-interaction-puzzles)
   - [Environmental Puzzles](#e-environmental-puzzles)
   - [Item Combination Puzzles](#f-item-combination-puzzles)
   - [Word/Spell Puzzles](#g-wordspell-puzzles)
   - [Timing Puzzles](#h-timing-puzzles)
   - [Container/Inventory Puzzles](#i-containerinventory-puzzles)
   - [Temple/Religious Puzzles](#j-templereligious-puzzles)
6. [Special Mechanics](#6-special-mechanics)

---

## 1. Combat System

Zork I has a **turn-based combat system** with strength calculations, weapons, and wound tracking.

### Combat Mechanics

From [1actions.zil:3375-3805](1actions.zil#L3375-L3805):

#### Player Strength Calculation

```python
# Global constants
STRENGTH_MIN = 1
STRENGTH_MAX = 5
SCORE_MAX = 350

def fight_strength():
    """Calculate player's combat strength"""
    # Base strength from score progression
    base = STRENGTH_MIN + (
        SCORE / (SCORE_MAX / (STRENGTH_MAX - STRENGTH_MIN))
    )

    # Modify by weapon strength
    if PRSI and PRSI.has_flag(WEAPONBIT):
        base += PRSI.strength_modifier

    # Modify by injuries (negative P_STRENGTH = wounded)
    if WINNER.strength < 0:
        base += WINNER.strength

    return max(0, base)
```

**Formula**: Strength increases as player earns points, making combat easier later in game.

#### Villain Table

```python
# Villains and their properties (1actions.zil:3801)
villains = [
    {
        "name": "TROLL",
        "weapon": SWORD,
        "base_strength": 1,
        "melee_action": troll_melee,
        "description": "nasty-looking troll"
    },
    {
        "name": "THIEF",
        "weapon": STILETTO,
        "base_strength": 1,
        "melee_action": thief_melee,
        "description": "seedy-looking individual"
    },
    {
        "name": "CYCLOPS",
        "weapon": None,
        "base_strength": 0,  # Actually 10000 when angry!
        "melee_action": cyclops_melee,
        "description": "large one-eyed creature"
    }
]
```

#### Combat Flow

From [1actions.zil:3476](1actions.zil#L3476):

```python
def hero_blow(villain):
    """Execute player's attack on villain"""

    # Check if player is stunned from previous blow
    if WINNER.has_flag(STAGGERED):
        print("You are still recovering from that last blow.")
        WINNER.remove_flag(STAGGERED)
        return

    # Calculate attack strength
    attack_strength = fight_strength()

    # Calculate villain defense
    defense_strength = villain_strength(villain)

    # Determine outcome using defense tables
    result = combat_result(attack_strength, defense_strength)

    # Apply weapon advantages/disadvantages
    if PRSI == SWORD and villain == TROLL:
        result = adjust_for_advantage(result)

    # Process result
    process_combat_result(result, villain)

    # Villain counter-attacks (if still alive)
    if villain.is_alive():
        villain_blow(villain)
```

#### Combat Result Types

```python
# Combat outcome constants (1actions.zil:3441-3449)
MISSED = 1          # Attack misses completely
UNCONSCIOUS = 2     # Defender knocked out
KILLED = 3          # Defender dies
LIGHT_WOUND = 4     # Reduces defense by 1
SERIOUS_WOUND = 5   # Reduces defense by 2
STAGGER = 6         # Next attack is ineffective
LOSE_WEAPON = 7     # Weapon dropped
HESITATE = 8        # No effect, just flavor text
SITTING_DUCK = 9    # Completely defenseless

# Defense result tables determine outcomes
def combat_result(attack, defense):
    """Determine combat outcome based on attack vs defense"""
    if defense > attack * 2:
        return random_choice(DEF1_RES)  # Strong defense
    elif defense > attack:
        return random_choice(DEF2_RES)  # Moderate defense
    else:
        return random_choice(DEF3_RES)  # Weak defense
```

#### Weapons

```python
# Weapons available in Zork I

class SWORD(Object):
    """Elvish sword - glows near evil"""
    location = LIVING_ROOM
    synonyms = ["SWORD", "BLADE", "ORCRIST"]
    adjectives = ["ELVISH"]
    flags = [TAKEBIT, WEAPONBIT]
    strength_modifier = 2  # Bonus to combat
    treasure_value = 0

    def on_near_evil(self):
        """Sword glows blue near troll"""
        if TROLL.location == HERE:
            print("Your sword is glowing with a faint blue glow.")

class KNIFE(Object):
    """Nasty knife - basic weapon"""
    location = ATTIC
    synonyms = ["KNIFE"]
    adjectives = ["NASTY"]
    flags = [TAKEBIT, WEAPONBIT]
    strength_modifier = 1

class STILETTO(Object):
    """Thief's stiletto - drops when thief defeated"""
    location = THIEF
    synonyms = ["KNIFE", "STILETTO"]
    adjectives = ["STILETTO"]
    flags = [TAKEBIT, WEAPONBIT]
    strength_modifier = 1

class AXE(Object):
    """Bloody axe - troll's weapon"""
    location = TROLL
    synonyms = ["AXE"]
    adjectives = ["BLOODY"]
    flags = [TAKEBIT, WEAPONBIT]
    strength_modifier = 2

class RUSTY_KNIFE(Object):
    """Cursed knife - causes suicide if used"""
    location = TEMPLE
    synonyms = ["KNIFE"]
    adjectives = ["RUSTY", "WICKED"]
    flags = [TAKEBIT, WEAPONBIT]

    def on_attack_with(self):
        """Cursed to slit own throat!"""
        jigs_up("The knife forces you to slit your own throat!")
```

#### Healing System

From [1actions.zil:3593](1actions.zil#L3593):

```python
# Injury and healing mechanics
CURE_WAIT = 30  # Ticks between healing increments

def i_cure():
    """Interrupt: Gradually heal player over time"""
    if WINNER.strength < 0:
        # Still injured
        WINNER.strength += 1

        # Reduce carrying capacity while injured
        LOAD_ALLOWED = 100 + (WINNER.strength * 10)

        if WINNER.strength == 0:
            # Fully healed
            print("You feel much better now.")
            LOAD_ALLOWED = 100
            return CURE_WAIT  # Stop healing interrupt

    return CURE_WAIT  # Continue healing

def apply_wound(damage):
    """Apply wound to player"""
    WINNER.strength -= damage
    LOAD_ALLOWED = 100 + (WINNER.strength * 10)

    if WINNER.strength <= -5:
        # Fatal injury
        jigs_up("You have died from your injuries.")
```

### Combat Death

```python
def check_combat_death():
    """Check if player died in combat"""
    if fight_strength() <= 0:
        jigs_up("You have been killed by the " + villain.description + ".")
```

---

## 2. Death Conditions

Zork I uses the **JIGS-UP** routine to handle all player deaths with a three-strikes system.

### JIGS-UP Death Handler

From [1actions.zil:4046](1actions.zil#L4046):

```python
# Global death tracking
DEATHS = 0  # Number of times player has died
DEAD = False  # Is player currently a spirit?
ALWAYS_LIT = False  # Spirit can see in dark

def jigs_up(death_message):
    """Handle player death"""
    global DEATHS, DEAD, ALWAYS_LIT

    # Print death message
    print(death_message)

    # Penalize score
    SCORE -= 10

    # Increment death counter
    DEATHS += 1

    if DEATHS == 1:
        # First death: Respawn in forest
        print("""
    ****    You have died    ****

As you take your last breath, you feel relieved of your burdens. The feeling
passes as you find yourself before the gates of Hell, where the spirits jeer
at you and deny you entry. Your senses are disturbed. The objects in the
dungeon appear indistinct, bleached of color, even unreal.
        """)

        # Become a spirit
        DEAD = True
        ALWAYS_LIT = True  # Can see in dark

        # Move to entrance to Hades
        move_player(ENTRANCE_TO_HADES)

        # Scatter items randomly
        randomize_objects()

    elif DEATHS == 2:
        # Second death: Can still be resurrected
        print("""
It appears that that last blow was too much for you. I'm afraid you are dead.
        """)
        DEAD = True
        move_player(ENTRANCE_TO_HADES)

    else:
        # Third death: Game over
        print("""
You clearly are a suicidal maniac. We don't allow psychotics in the
cave, since they may harm other adventurers. Your remains will be
installed in the Land of the Living Dead, where your fellow
adventurers may gloat over them.
        """)

        # Game over
        game_over()
```

### Spirit World Mechanics

```python
def spirit_world_behavior():
    """Special rules when DEAD = True"""

    # Can see in darkness
    if DEAD:
        ALWAYS_LIT = True

    # Objects appear bleached
    def describe_as_spirit(obj):
        """Spirit vision shows objects differently"""
        return f"The {obj.description} appears bleached and unreal."

    # Cannot interact with most objects
    def can_take_as_spirit(obj):
        """Spirits cannot take objects"""
        print("Your hand passes through the object.")
        return False
```

### Resurrection

From [1actions.zil:3152](1actions.zil#L3152):

```python
def v_pray():
    """PRAY verb - can resurrect if in temple"""
    if HERE == SOUTH_TEMPLE and DEAD:
        # Resurrection!
        DEAD = False
        ALWAYS_LIT = False
        WINNER.strength = 0  # Restore health

        print("""
From the distance the sound of a lone trumpet is heard. The room becomes
very bright and you feel disembodied. In a moment, the feeling passes.
You are whole once again.
        """)

        # Teleport to forest
        move_player(FOREST_1)
    else:
        print("Your prayers are not heard.")
```

### Specific Death Scenarios

#### 1. Grue Death (Darkness)

```python
def check_grue_attack():
    """Called each turn in darkness without light"""
    if not is_lit() and not ALWAYS_LIT:
        if random(100) < 30:  # 30% chance each turn
            jigs_up("""
    ****    You have been eaten by a grue    ****

Oh, no! You have walked into the slavering fangs of a lurking grue!
            """)
```

#### 2. Troll Death

```python
class TROLL(Object):
    location = TROLL_ROOM
    flags = [ACTORBIT, FIGHTBIT]
    strength = 1

    def on_death(self):
        """When troll dies, open passages"""
        global TROLL_FLAG
        TROLL_FLAG = True

        print("The troll's body dissolves into a cloud of black smoke.")

        # Open blocked passages
        TROLL_ROOM.remove_exit_blocks()
```

#### 3. Drowning Deaths

```python
def check_drowning_river():
    """Drowning in river without boat"""
    if HERE.has_flag(WATERBIT) and not in_vehicle(BOAT):
        jigs_up("You have drowned in the swift currents.")

def check_reservoir_flood():
    """Reservoir flooding when gates open"""
    if HERE == RESERVOIR and GATES_OPEN and not LOW_TIDE:
        jigs_up("The water rises rapidly and you drown!")
```

#### 4. Fall Deaths

```python
def rainbow_collapse():
    """Stepping on insubstantial rainbow"""
    if not RAINBOW_FLAG:  # Rainbow not solid
        jigs_up("""
You have attempted to cross the rainbow when it is not solid. You
plummet several hundred feet to your death.
        """)

def chasm_fall():
    """Falling into chasm"""
    jigs_up("You fall into the chasm and are killed.")
```

#### 5. Cursed Object Deaths

```python
# Rusty knife curse
def rusty_knife_attack():
    """Using cursed knife"""
    jigs_up("""
The knife, which has always been blade up, is now blade down. As you
attempt to remove it, the blade turns towards you as if it had a will of
its own. The wicked blade takes your hand, then your arm, and finally
all of you. You have been killed by the knife.
    """)

# Skeleton desecration
def disturb_skeleton():
    """Disturbing skeleton in temple"""
    jigs_up("""
The ghost of the desecrated skeleton appears and curses you, saying
\"You have violated my rest. Now you shall join me in eternal torment!\"
You feel your life force draining away...
    """)

# Burning black book
def burn_black_book():
    """Setting black book on fire"""
    jigs_up("""
A booming voice says \"Wrong, cretin!\" and you notice that you have
turned into a pile of dust.
    """)
```

#### 6. Environmental Hazards

```python
def beach_digging_collapse():
    """Digging too deep at beach"""
    if BEACH_DIG >= 4:
        jigs_up("The hole collapses, smothering you.")

def volcano_death():
    """Various volcanic hazards"""
    jigs_up("You have been incinerated by molten lava.")
```

#### 7. Combat Deaths

```python
def combat_death(villain):
    """Death in combat"""
    messages = {
        TROLL: "The troll's axe removes your head.",
        THIEF: "The thief's stiletto finds your heart.",
        CYCLOPS: "The cyclops crushes you like an insect."
    }
    jigs_up(messages[villain])
```

### Death Summary Table

| Death Type | Trigger | Avoidable? | Notes |
|------------|---------|------------|-------|
| **Grue** | Darkness without light | Yes | 30% chance/turn in dark |
| **Troll combat** | Losing to troll | Yes | Can flee or win combat |
| **Thief combat** | Losing to thief | Yes | Can flee or win combat |
| **Cyclops combat** | Losing to cyclops | Yes | Very strong - use puzzle |
| **Drowning (river)** | Swimming without boat | Yes | Use inflatable boat |
| **Drowning (reservoir)** | Water rising | Yes | Control dam gates |
| **Rainbow fall** | Insubstantial rainbow | Yes | Wave sceptre to solidify |
| **Chasm fall** | Various locations | Yes | Don't jump/fall |
| **Beach collapse** | 4th dig at beach | Yes | Stop at 3 digs |
| **Rusty knife** | Using cursed knife | Yes | Don't attack with it |
| **Skeleton curse** | Disturbing skeleton | Yes | Don't desecrate body |
| **Black book burn** | Burning book | Yes | Don't burn the book |
| **Self-attack** | ATTACK ME WITH weapon | Yes | Don't do this! |

---

## 3. Win Condition

### Maximum Score: 350 Points

From [gverbs.zil:1851-1865](gverbs.zil#L1851-L1865):

```python
# Global win tracking
WON_FLAG = False
SCORE_MAX = 350

def check_win_condition():
    """Called after each score change"""
    if SCORE == 350 and not WON_FLAG:
        WON_FLAG = True

        # Make map visible in trophy case
        MAP.remove_flag(INVISIBLE)

        # Remove barrier to barrow
        WEST_OF_HOUSE.remove_flag(TOUCHBIT)

        # Whisper hint
        print("""
An almost inaudible voice whispers in your ear, \"Look to your
treasures for the final secret.\"
        """)
```

### Final Sequence

From [1actions.zil:403](1actions.zil#L403):

```python
class STONE_BARROW(Room):
    """Final room - triggers endgame"""

    def on_enter(self):
        """Entering triggers victory"""
        if WON_FLAG:
            print("""
Congratulations! You have completed ZORK: The Great Underground Empire.
You have scored 350 out of a possible 350 points, using """ + str(MOVES) + """ moves.
This gives you the rank of Master Adventurer.

Your feat has been noted and you may now continue your adventure in
ZORK II: The Wizard of Frobozz and ZORK III: The Dungeon Master.
            """)

            game_over(victory=True)
```

### Scoring System

```python
def score_treasure(treasure):
    """Award points for placing treasure in trophy case"""

    # Move treasure to trophy case
    treasure.location = TROPHY_CASE

    # Award points (TVALUE = treasure value)
    points = treasure.treasure_value
    SCORE += points

    print(f"Your score has just gone up by {points} points.")

    # Check for win
    check_win_condition()
```

### Trophy Case

From [1actions.zil:442](1actions.zil#L442):

```python
class TROPHY_CASE(Object):
    """Trophy case - scores treasures placed inside"""
    location = LIVING_ROOM
    synonyms = ["CASE", "TROPHY"]
    adjectives = ["TROPHY"]
    description = "trophy case"
    flags = [TRANSBIT, CONTBIT, OPENBIT, NDESCBIT]
    capacity = 10000  # Can hold many treasures

    def on_object_inserted(self, obj):
        """Score when treasure placed inside"""
        if obj.treasure_value > 0:
            score_treasure(obj)
```

---

## 4. Treasure List

All 19 treasures in Zork I that count toward the 350-point maximum.

From [1dungeon.zil](1dungeon.zil):

```python
# Complete treasure list with values and locations

treasures = [
    {
        "name": "JEWELED_EGG",
        "value": 5,
        "location": "BIRDS_NEST (in UP-A-TREE)",
        "notes": "Contains wind-up canary, fragile"
    },
    {
        "name": "CLOCKWORK_CANARY",
        "value": 4,
        "location": "Inside EGG",
        "notes": "Wind it in forest to attract songbird"
    },
    {
        "name": "BAUBLE",
        "value": 1,
        "location": "Dropped by songbird",
        "notes": "Reward for canary puzzle"
    },
    {
        "name": "BRASS_LANTERN",
        "value": 0,
        "location": "LIVING_ROOM (on table)",
        "notes": "Not a treasure, but essential item"
    },
    {
        "name": "ELVISH_SWORD",
        "value": 0,
        "location": "LIVING_ROOM (above mantle)",
        "notes": "Not a treasure, weapon"
    },
    {
        "name": "BAG_OF_COINS",
        "value": 10,
        "location": "MAZE_5",
        "notes": "Leather bag, contains zorkmids"
    },
    {
        "name": "CRYSTAL_SKULL",
        "value": 10,
        "location": "LAND_OF_LIVING_DEAD",
        "notes": "Crystal, beautifully carved"
    },
    {
        "name": "COFFIN",
        "value": 15,
        "location": "EGYPT_ROOM",
        "notes": "Contains SCEPTRE, treasure itself"
    },
    {
        "name": "SCEPTRE",
        "value": 6,
        "location": "Inside COFFIN",
        "notes": "Makes rainbow solid, also weapon"
    },
    {
        "name": "POT_OF_GOLD",
        "value": 10,
        "location": "END_OF_RAINBOW",
        "notes": "Appears when rainbow solidified"
    },
    {
        "name": "PAINTING",
        "value": 6,
        "location": "GALLERY",
        "notes": "Beautiful painting, can be destroyed (value=0)"
    },
    {
        "name": "TRIDENT",
        "value": 4,
        "location": "CORAL_ROOM",
        "notes": "Crystal trident"
    },
    {
        "name": "CHALICE",
        "value": 10,
        "location": "TREASURE_ROOM",
        "notes": "Jewel-encrusted egg-shaped chalice, thief steals"
    },
    {
        "name": "TRUNK_OF_JEWELS",
        "value": 15,
        "location": "RESERVOIR (bottom)",
        "notes": "Large trunk, requires LOW_TIDE"
    },
    {
        "name": "EMERALD",
        "value": 10,
        "location": "Inside BUOY",
        "notes": "Large emerald, hidden in buoy on river"
    },
    {
        "name": "JADE_FIGURINE",
        "value": 5,
        "location": "BAT_ROOM",
        "notes": "Jade figurine"
    },
    {
        "name": "PLATINUM_BAR",
        "value": 10,
        "location": "LOUD_ROOM",
        "notes": "Loud room makes taking difficult"
    },
    {
        "name": "DIAMOND",
        "value": 10,
        "location": "WEST_OF_CHASM",
        "notes": "Huge diamond"
    },
    {
        "name": "SCARAB",
        "value": 5,
        "location": "SANDY_BEACH (buried)",
        "notes": "Dig 3 times with shovel to reveal"
    }
]

# Total: 350 points when all placed in trophy case
```

### Treasure Scoring Example

```python
# Example: Placing skull in trophy case
class SKULL(Object):
    location = LAND_OF_LIVING_DEAD
    synonyms = ["SKULL", "HEAD"]
    adjectives = ["CRYSTAL"]
    description = "crystal skull"
    treasure_value = 10  # Worth 10 points
    size = 4
    flags = [TAKEBIT]

    first_desc = """
Lying in one corner of the room is a beautifully carved crystal skull.
It appears to be grinning at you rather nastily.
    """

# Player actions:
# > TAKE SKULL
# Taken.
# > GO TO LIVING ROOM
# > PUT SKULL IN TROPHY CASE
# Your score has just gone up by 10 points.
# Score: 10/350
```

---

## 5. Puzzle Types

### A. Lock and Key Puzzles

#### 1. Grate Puzzle

From [1actions.zil:850](1actions.zil#L850):

**Setup:**
- GRATE blocks entrance to underground
- Requires SKELETON KEYS to unlock
- LEAVES conceal grate from above

**Solution:**

```python
# Global lock state
GRUNLOCK = False  # False = locked, True = unlocked

class GRATE(Object):
    location = GRATING_ROOM
    synonyms = ["GRATE", "GRATING"]
    description = "metal grate"
    flags = [DOORBIT, NDESCBIT]
    action = grate_fcn

def grate_fcn():
    """Handle grate interactions"""

    if current_verb == "UNLOCK":
        if HERE == GRATING_ROOM and PRSI == KEYS:
            global GRUNLOCK
            GRUNLOCK = True
            print("The grate is unlocked.")
        elif HERE == GRATING_CLEARING:
            print("You can't reach the lock from here.")
        else:
            print(f"Can you unlock a grating with a {PRSI.desc}?")

    elif current_verb == "LOCK":
        if HERE == GRATING_ROOM:
            GRUNLOCK = False
            print("The grate is locked.")
        else:
            print("You can't lock it from this side.")

    elif current_verb in ["OPEN", "CLOSE"]:
        if GRUNLOCK:
            open_close(GRATE,
                      "The grating opens.",
                      "The grating is closed.")
        else:
            print("The grating is locked.")

# Keys object
class KEYS(Object):
    location = MAZE_5
    synonyms = ["KEY", "KEYS"]
    adjectives = ["SKELETON"]
    description = "skeleton key"
    flags = [TAKEBIT, TOOLBIT]
```

**Player Experience:**
```
> MOVE LEAVES
The pile of leaves is moved, revealing a grating.

> OPEN GRATE
The grating is locked.

> (find keys in maze)
> UNLOCK GRATE WITH KEYS
The grate is unlocked.

> OPEN GRATE
The grating opens, revealing a dark passage.
```

#### 2. Trap Door Puzzle

From [1actions.zil:504](1actions.zil#L504):

**Setup:**
- Trap door in LIVING-ROOM concealed by RUG
- Leads down to CELLAR
- Locks from above when closed

**Solution:**

```python
class TRAP_DOOR(Object):
    location = LIVING_ROOM
    synonyms = ["DOOR", "TRAPDOOR"]
    adjectives = ["TRAP"]
    description = "trap door"
    flags = [DOORBIT, NDESCBIT, INVISIBLE]
    action = trap_door_fcn

class RUG(Object):
    location = LIVING_ROOM
    synonyms = ["RUG", "CARPET"]
    adjectives = ["ORIENTAL", "TREASURES"]
    description = "oriental rug"
    flags = []  # Cannot be taken

    def on_move(self):
        """Moving rug reveals trap door"""
        TRAP_DOOR.remove_flag(INVISIBLE)
        print("With a great effort, the rug is moved to one side, revealing a closed trap door.")

def trap_door_fcn():
    """One-way lock mechanism"""

    # From above (Living Room) - can open/close
    if current_verb in ["OPEN", "CLOSE"] and HERE == LIVING_ROOM:
        open_close(PRSO,
                  "The door reluctantly opens to reveal a rickety staircase descending into darkness.",
                  "The door swings shut and closes.")

    # From below (Cellar) - LOCKED!
    elif HERE == CELLAR:
        if current_verb in ["OPEN", "UNLOCK"] and not TRAP_DOOR.has_flag(OPENBIT):
            print("The door is locked from above.")
```

**Player Experience:**
```
> MOVE RUG
With a great effort, the rug is moved to one side, revealing a closed trap door.

> OPEN TRAP DOOR
The door reluctantly opens to reveal a rickety staircase descending into darkness.

> DOWN
Cellar
You are in a dark and damp cellar.

> UP
The trap door is closed.

> OPEN DOOR
The door is locked from above.

(Must use chimney or stay trapped!)
```

#### 3. Front Door Puzzle

From [1actions.zil:2163](1actions.zil#L2163):

**Setup:**
- Front door of white house
- Permanently locked - boarded shut
- RED HERRING - cannot be opened!

```python
class FRONT_DOOR(Object):
    location = WEST_OF_HOUSE
    synonyms = ["DOOR"]
    adjectives = ["FRONT"]
    description = "front door"
    flags = [DOORBIT, NDESCBIT]
    action = front_door_fcn

def front_door_fcn():
    """Permanently locked door"""
    if current_verb == "OPEN":
        print("The door cannot be opened.")
    elif current_verb == "BURN":
        print("You cannot burn this door.")
    elif current_verb == "KNOCK":
        print("No one answers.")
```

### B. Light/Darkness Puzzles

Light management is **critical** - darkness = grue death!

#### 1. Brass Lantern (Primary Light Source)

From [1actions.zil:2230](1actions.zil#L2230):

```python
class LAMP(Object):
    """Battery-powered brass lantern"""
    location = LIVING_ROOM
    synonyms = ["LAMP", "LANTERN", "LIGHT"]
    adjectives = ["BRASS"]
    description = "brass lantern"
    flags = [TAKEBIT, LIGHTBIT]
    capacity = 200  # Battery life in turns

    def on_turn_on(self):
        """Turn on lamp"""
        if self.has_flag(RMUNGBIT):
            # Battery dead!
            print("The lamp has run out of power.")
            return False

        self.add_flag(ONBIT)
        print("The brass lantern is now on.")

        # Start battery countdown
        start_interrupt(i_lantern)

# Battery countdown interrupt
LAMP_TURNS = 200  # Remaining battery life

def i_lantern():
    """Track lamp battery drain"""
    global LAMP_TURNS

    if LAMP.has_flag(ONBIT):
        LAMP_TURNS -= 1

        # Warnings
        if LAMP_TURNS == 100:
            print("The lamp appears a bit dimmer.")
        elif LAMP_TURNS == 70:
            print("The lamp is definitely dimmer now.")
        elif LAMP_TURNS == 15:
            print("The lamp is nearly out!")
        elif LAMP_TURNS == 0:
            # Battery dead!
            LAMP.add_flag(RMUNGBIT)
            LAMP.remove_flag(ONBIT)
            print("The lamp has run out of power.")
            return -1  # Stop interrupt

    return 1  # Continue next turn
```

**Player Experience:**
```
> TURN ON LAMP
The brass lantern is now on.

... (after 100 turns) ...
The lamp appears a bit dimmer.

... (after 170 turns) ...
The lamp is definitely dimmer now.

... (after 185 turns) ...
The lamp is nearly out!

... (after 200 turns) ...
The lamp has run out of power.
It is pitch black. You are likely to be eaten by a grue.
```

#### 2. Matches (Limited Resource)

From [1actions.zil:2278](1actions.zil#L2278):

```python
class MATCHES(Object):
    """Book of matches - finite resource"""
    location = LIVING_ROOM
    synonyms = ["MATCH", "MATCHES", "BOOK"]
    adjectives = ["MATCH"]
    description = "book of matches"
    flags = [TAKEBIT, LIGHTBIT, FLAMEBIT]
    match_count = 20  # Number of matches remaining

    def on_light(self):
        """Light a match"""
        if self.match_count > 0:
            self.match_count -= 1
            self.add_flag(ONBIT)
            print(f"One of the matches starts to burn. ({self.match_count} matches left)")

            # Match burns for 2 turns
            start_interrupt(i_match, duration=2)
        else:
            print("The book is empty.")

def i_match():
    """Match burns out after 2 turns"""
    MATCHES.remove_flag(ONBIT)
    print("The match has gone out.")
    return -1  # Stop interrupt
```

**Player Experience:**
```
> LIGHT MATCH
One of the matches starts to burn. (19 matches left)

> WAIT
Time passes.

> WAIT
The match has gone out.
```

#### 3. Candles (Medium Duration)

From [1actions.zil:2290](1actions.zil#L2290):

```python
class CANDLES(Object):
    """Pair of candles on altar"""
    location = SOUTH_TEMPLE
    synonyms = ["CANDLE", "CANDLES"]
    adjectives = ["PAIR"]
    description = "pair of candles"
    flags = [TAKEBIT, LIGHTBIT, BURNBIT]
    burn_time = 40  # Turns before burning out

    def on_light(self):
        """Light candles"""
        if not self.has_flag(RMUNGBIT):
            self.add_flag(ONBIT)
            print("The candles are lit.")
            start_interrupt(i_candles)
        else:
            print("The candles have burned down to stubs.")

def i_candles():
    """Candles burn for 40 turns"""
    global burn_time

    if CANDLES.has_flag(ONBIT):
        burn_time -= 1

        if burn_time == 10:
            print("The candles are getting quite short.")
        elif burn_time == 0:
            CANDLES.remove_flag(ONBIT)
            CANDLES.add_flag(RMUNGBIT)
            print("The candles have burned down completely.")
            return -1

    return 1
```

#### 4. Torch (Permanent Light)

From [1actions.zil:944](1actions.zil#L944):

```python
class TORCH(Object):
    """Eternally burning torch - cannot be extinguished"""
    location = TORCH_ROOM
    synonyms = ["TORCH"]
    adjectives = ["BURNING"]
    description = "torch"
    flags = [TAKEBIT, LIGHTBIT, ONBIT, FLAMEBIT, BURNBIT]

    def on_extinguish(self):
        """Cannot turn off!"""
        print("You nearly burn your hand trying.")
        return False
```

### C. Water Puzzles

#### 1. Dam/Reservoir System

From [1actions.zil:1400](1actions.zil#L1400):

**Complex puzzle** controlling water flow and access to areas.

```python
# Global water state
GATES_OPEN = False  # Dam gates state
LOW_TIDE = True     # Reservoir water level

class DAM(Object):
    """Flood Control Dam #3"""
    location = DAM_ROOM
    synonyms = ["DAM"]
    description = "dam"
    flags = [NDESCBIT]

class BOLT(Object):
    """Controls dam gates"""
    location = DAM_ROOM
    synonyms = ["BOLT"]
    adjectives = ["YELLOW"]
    description = "yellow bolt"
    flags = [NDESCBIT]
    action = bolt_fcn

def bolt_fcn():
    """Turn bolt to control gates"""
    global GATES_OPEN, LOW_TIDE

    if current_verb == "TURN":
        if not GATES_OPEN:
            # Open gates
            GATES_OPEN = True
            print("The sluice gates open and water pours through the dam.")

            # Start water flow
            start_interrupt(i_water_flow)
        else:
            # Close gates
            GATES_OPEN = False
            print("The sluice gates close.")

def i_water_flow():
    """Water drains from reservoir"""
    global LOW_TIDE

    if GATES_OPEN:
        # Water draining
        if not LOW_TIDE:
            # After 4 turns, reservoir empty
            LOW_TIDE = True
            print("The water level in the reservoir has dropped.")

            # Make TRUNK_OF_JEWELS accessible
            TRUNK_OF_JEWELS.location = RESERVOIR
            TRUNK_OF_JEWELS.remove_flag(INVISIBLE)
    else:
        # Water filling
        if LOW_TIDE:
            # After 4 turns, reservoir full
            LOW_TIDE = False
            print("The water level in the reservoir has risen.")

            # Hide trunk underwater
            TRUNK_OF_JEWELS.add_flag(INVISIBLE)
```

**Solution Sequence:**
```
> TURN BOLT
The sluice gates open and water pours through the dam.

> WAIT
Time passes.

> WAIT
Time passes.

> WAIT
Time passes.

> WAIT
The water level in the reservoir has dropped.

> GO TO RESERVOIR
Reservoir
The water level is quite low here, exposing a sandy bottom.
There is a trunk of jewels here.

> TAKE TRUNK
Taken.
```

#### 2. Bottle and Water

From [1actions.zil:1491](1actions.zil#L1491):

```python
class BOTTLE(Object):
    """Glass bottle - container for water"""
    location = KITCHEN
    synonyms = ["BOTTLE"]
    adjectives = ["GLASS"]
    description = "glass bottle"
    flags = [TAKEBIT, CONTBIT, TRANSBIT]
    capacity = 5

class WATER(Object):
    """Water - initially in bottle"""
    location = BOTTLE
    synonyms = ["WATER", "H2O", "QUANTITY"]
    adjectives = ["SMALL"]
    description = "quantity of water"
    flags = []

    def on_pour(self):
        """Pour water out"""
        if PRSI:
            print(f"The water spills over the {PRSI.desc}.")
        else:
            print("The water evaporates.")

        # Water disappears
        self.remove_from_game()

# Used in cyclops puzzle
def give_water_to_cyclops():
    """Cyclops drinks water after hot lunch"""
    if PRSO == WATER and PRSI == CYCLOPS:
        if CYCLOPS.has_flag(SPICYBIT):  # Ate lunch
            print("The cyclops gulps down the water and falls into a deep sleep.")
            CYCLOPS_FLAG = True
            CYCLOPS.add_flag(ASLEEPBIT)
        else:
            print("The cyclops ignores you.")
```

#### 3. Boat Puzzle

From [1actions.zil:2652](1actions.zil#L2652):

**Multi-state object** for river navigation.

```python
class INFLATABLE_BOAT(Object):
    """Boat in deflated state"""
    location = DAM_BASE
    synonyms = ["BOAT", "RAFT"]
    adjectives = ["INFLATABLE", "MAGIC"]
    description = "magic boat"
    flags = [TAKEBIT, BURNBIT]
    action = boat_fcn

class INFLATED_BOAT(Object):
    """Boat in inflated state"""
    synonyms = ["BOAT", "RAFT"]
    description = "magic boat"
    flags = [VEHBIT, BURNBIT, OPENBIT, SEARCHBIT, CONTBIT]
    capacity = 100

    def on_board(self):
        """Enter boat"""
        WINNER.location = self
        print("You are now in the magic boat.")

class PUNCTURED_BOAT(Object):
    """Boat with hole - sinks!"""
    description = "punctured boat"

    def on_float(self):
        """Sinks if on water"""
        jigs_up("The boat deflates and you drown!")

def boat_fcn():
    """Handle boat inflation/deflation"""
    if current_verb == "INFLATE":
        if PRSI == PUMP or PUMP in inventory():
            # Transform to inflated boat
            INFLATABLE_BOAT.remove_from_game()
            INFLATED_BOAT.location = HERE
            print("The boat inflates and appears seaworthy.")
        else:
            print("With what?")

    elif current_verb == "DEFLATE":
        if INFLATED_BOAT in HERE:
            # Transform back
            INFLATED_BOAT.remove_from_game()
            INFLATABLE_BOAT.location = HERE
            print("The boat deflates.")

class PUMP(Object):
    """Hand pump for boat"""
    location = DAM_BASE
    synonyms = ["PUMP"]
    adjectives = ["HAND", "BICYCLE"]
    description = "hand-held air pump"
    flags = [TAKEBIT, TOOLBIT]

class PUTTY(Object):
    """Repairs punctured boat"""
    location = None
    synonyms = ["PUTTY", "GLUE"]
    description = "tube of putty"
    flags = [TAKEBIT]

    def on_apply_to_boat(self):
        """Repair puncture"""
        if target == PUNCTURED_BOAT:
            PUNCTURED_BOAT.remove_from_game()
            INFLATED_BOAT.location = HERE
            print("The boat is repaired.")
```

### D. NPC Interaction Puzzles

#### 1. Troll Puzzle

From [1actions.zil:640](1actions.zil#L640):

**Multiple solutions** - combat, food, or weapon gift.

```python
class TROLL(Object):
    """Nasty troll blocking passages"""
    location = TROLL_ROOM
    synonyms = ["TROLL"]
    adjectives = ["NASTY"]
    description = "nasty-looking troll"
    flags = [ACTORBIT, FIGHTBIT]
    strength = 1
    action = troll_fcn

def troll_fcn():
    """Troll behaviors"""

    # Solution 1: Combat
    if current_verb == "ATTACK":
        # Normal combat routine
        start_combat(TROLL)

    # Solution 2: Give food
    elif current_verb == "GIVE" and PRSO.has_flag(FOODBIT):
        # 20% chance eating weapon kills troll
        if PRSO.has_flag(WEAPONBIT) and random(100) < 20:
            print("""
The troll, who is not overly proud, graciously accepts the gift
and eats it hungrily. As he finishes, he suddenly grasps his chest,
gasps for breath, and collapses. Internal bleeding, no doubt.
            """)
            kill_troll()
        else:
            print("The troll, who is remarkably coordinated, catches it and, not  being hungry, throws it back.")
            # Throws it back!
            PRSO.location = HERE

    # Solution 3: Give axe back
    elif current_verb == "GIVE" and PRSO == AXE:
        print("The troll accepts the axe... which is, after all, his.")
        TROLL.add_flag(WEAPONBIT)
        AXE.location = TROLL

    # Sword glows near troll
    if SWORD in inventory() or SWORD.location == HERE:
        print("Your sword is glowing with a faint blue glow.")

def kill_troll():
    """Troll death opens passages"""
    global TROLL_FLAG
    TROLL_FLAG = True
    TROLL.remove_from_game()

    # Open blocked passages in troll room
    TROLL_ROOM.add_exit(EAST, MAZE_1)
    TROLL_ROOM.add_exit(NORTH, NORTH_SOUTH_PASSAGE)
```

**Player Experience (Combat):**
```
> ATTACK TROLL WITH SWORD
The troll swings his axe, but misses.
You stab the troll with your sword. The troll looks wounded.

> ATTACK TROLL WITH SWORD
You swing wildly and connect! The troll is killed!
The troll's body dissolves into a cloud of black smoke.
```

**Player Experience (Food Trick):**
```
> THROW LUNCH AT TROLL
The troll, who is not overly proud, graciously accepts the gift
and eats it hungrily. As he finishes, he suddenly grasps his chest,
gasps for breath, and collapses. Internal bleeding, no doubt.
```

#### 2. Thief Puzzle

From [1actions.zil:1800](1actions.zil#L1800):

**Steals treasures** - must be defeated to recover them.

```python
class THIEF(Object):
    """Seedy-looking thief"""
    location = None  # Appears randomly
    synonyms = ["THIEF", "ROBBER", "CRIMINAL"]
    description = "seedy-looking individual"
    flags = [ACTORBIT, FIGHTBIT]
    strength = 1

    def on_tick(self):
        """Appears, steals, disappears"""
        if random(100) < 30:  # 30% chance per turn
            self.location = HERE
            print("A seedy-looking individual appears from the shadows!")

            # Try to steal
            attempt_theft()

def attempt_theft():
    """Thief steals valuable objects"""
    # Find stealable objects
    targets = [obj for obj in HERE.children
               if obj.treasure_value > 0 or obj in inventory()]

    if targets and random(100) < 70:  # 70% success rate
        stolen = random_choice(targets)
        stolen.location = THIEF
        stolen.add_flag(INVISIBLE)

        print(f"The thief steals the {stolen.desc} and disappears into the darkness!")
        THIEF.location = None
    else:
        print("The thief looks around nervously and flees.")
        THIEF.location = None

def defeat_thief():
    """Defeating thief recovers stolen items"""
    print("The thief is killed. His booty is left behind.")

    # Move all stolen items to TREASURE_ROOM
    for obj in THIEF.children:
        obj.location = TREASURE_ROOM
        obj.remove_flag(INVISIBLE)

    THIEF.remove_from_game()
```

#### 3. Cyclops Puzzle

From [1actions.zil:1515](1actions.zil#L1515):

**Multiple solutions** - magic word, food combo, or impossible combat.

```python
class CYCLOPS(Object):
    """Huge one-eyed creature guarding treasury"""
    location = CYCLOPS_ROOM
    synonyms = ["CYCLOPS", "GIANT", "MONSTER"]
    adjectives = ["ONE-EYED", "HUGE"]
    description = "cyclops"
    flags = [ACTORBIT]
    strength = 10000  # Nearly impossible to defeat!
    action = cyclops_fcn

# Cyclops anger level
CYCLOWRATH = 0

def cyclops_fcn():
    """Cyclops puzzle solutions"""

    # Solution 1: Say "ODYSSEUS"
    if current_verb == "SAY" and word == "ODYSSEUS":
        print("""
The cyclops, hearing the name of his father's nemesis, flees the room
in terror, forgetting to take his axe.
        """)

        # Cyclops flees
        CYCLOPS.location = None

        # Opens east wall to Treasury
        global MAGIC_FLAG
        MAGIC_FLAG = True
        CYCLOPS_ROOM.add_exit(EAST, TREASURY)

    # Solution 2: Give lunch + water combo
    elif current_verb == "GIVE":
        if PRSO == LUNCH:
            print("The cyclops eagerly devours the hot peppers and begins to breathe fire!")
            CYCLOPS.add_flag(SPICYBIT)
            global CYCLOWRATH
            CYCLOWRATH = 2  # Will attack in 2 turns
            start_interrupt(i_cyclops)

        elif PRSO == WATER and CYCLOPS.has_flag(SPICYBIT):
            print("The cyclops gulps the water and curls up in the corner, asleep.")
            global CYCLOPS_FLAG
            CYCLOPS_FLAG = True
            MAGIC_FLAG = True
            CYCLOPS_ROOM.add_exit(EAST, TREASURY)

    # Solution 3: Combat (nearly impossible!)
    elif current_verb == "ATTACK":
        if CYCLOPS.strength > fight_strength() * 100:
            jigs_up("The cyclops crushes you like an insect.")

def i_cyclops():
    """Cyclops countdown to attack"""
    global CYCLOWRATH

    if CYCLOWRATH > 0:
        CYCLOWRATH -= 1
        if CYCLOWRATH == 0:
            # Attacks!
            print("The cyclops, enraged by the burning sensation, attacks!")
            start_combat(CYCLOPS)
```

**Solution 1 (Magic Word):**
```
> SAY "ODYSSEUS"
The cyclops, hearing the name of his father's nemesis, flees the room
in terror, forgetting to take his axe.
```

**Solution 2 (Food Combo):**
```
> GIVE LUNCH TO CYCLOPS
The cyclops eagerly devours the hot peppers and begins to breathe fire!

> GIVE WATER TO CYCLOPS
The cyclops gulps the water and curls up in the corner, asleep.
```

#### 4. Ghost/Spirit Puzzle

From [1actions.zil:264](1actions.zil#L264):

**Bell, Book, and Candle exorcism** - specific sequence required.

```python
class GHOSTS(Object):
    """Evil spirits haunting temple"""
    location = ENTRANCE_TO_HADES
    synonyms = ["GHOST", "GHOSTS", "SPIRIT", "SPIRITS"]
    description = "evil spirits"
    flags = [NDESCBIT]

    def can_attack(self):
        """Cannot attack ghosts"""
        print("The spirits are incorporeal and cannot be harmed.")
        return False

# Required items
class BELL(Object):
    location = TEMPLE
    synonyms = ["BELL"]
    adjectives = ["BRASS"]
    description = "brass bell"
    flags = [TAKEBIT]

class BLACK_BOOK(Object):
    location = TEMPLE
    synonyms = ["BOOK"]
    adjectives = ["BLACK"]
    description = "black book"
    flags = [TAKEBIT, READBIT, BURNBIT]
    text = "Turn to page 569 for a ritual of exorcism."

class CANDLES(Object):
    # (defined earlier)
    pass

def exorcism_ritual():
    """Perform bell, book, and candle ritual"""

    # Check all items present
    if BELL not in inventory():
        return False
    if BLACK_BOOK not in inventory():
        return False
    if CANDLES not in inventory() or not CANDLES.has_flag(ONBIT):
        return False

    # Sequence:
    # 1. Ring bell
    if current_verb == "RING" and PRSO == BELL:
        print("The bell chimes ominously.")
        ritual_step_1 = True

    # 2. Read book (page 569)
    if current_verb == "READ" and PRSO == BLACK_BOOK:
        print("The book describes a ceremony of exorcism.")
        ritual_step_2 = True

    # 3. Light candles
    if CANDLES.has_flag(ONBIT):
        ritual_step_3 = True

    # All steps complete?
    if ritual_step_1 and ritual_step_2 and ritual_step_3:
        print("""
The spirits, sensing a presence of great good, flee the room in terror!
The temple is now at peace.
        """)

        GHOSTS.remove_from_game()
        # Temple areas now safe
```

### E. Environmental Puzzles

#### 1. Rainbow Bridge

From [1actions.zil:2634](1actions.zil#L2634):

**Wave sceptre to solidify** rainbow, allowing crossing.

```python
class RAINBOW(Object):
    """Rainbow bridge at waterfall"""
    location = ARAGAIN_FALLS
    synonyms = ["RAINBOW", "BRIDGE"]
    description = "rainbow"
    flags = [NDESCBIT]
    action = rainbow_fcn

# Rainbow state
RAINBOW_FLAG = False  # False = insubstantial, True = solid

def rainbow_fcn():
    """Rainbow interactions"""

    if current_verb == "CROSS":
        if RAINBOW_FLAG:
            # Solid - can cross
            do_walk(WEST)  # To END_OF_RAINBOW
        else:
            # Insubstantial - death!
            jigs_up("""
You have attempted to cross the rainbow when it is not solid. You
plummet several hundred feet to your death.
            """)

class SCEPTRE(Object):
    """Makes rainbow solid when waved"""
    location = COFFIN
    synonyms = ["SCEPTRE", "SCEPTER"]
    adjectives = ["CRYSTAL"]
    description = "crystal sceptre"
    treasure_value = 6
    flags = [TAKEBIT, WEAPONBIT]
    action = sceptre_fcn

def sceptre_fcn():
    """Wave sceptre to control rainbow"""
    global RAINBOW_FLAG

    if current_verb == "WAVE":
        if HERE == ARAGAIN_FALLS or HERE == END_OF_RAINBOW:
            if not RAINBOW_FLAG:
                # Solidify rainbow
                RAINBOW_FLAG = True
                print("Suddenly, the rainbow appears to become solid!")

                # Pot of gold appears
                POT_OF_GOLD.location = END_OF_RAINBOW
                POT_OF_GOLD.remove_flag(INVISIBLE)
            else:
                # Already solid
                print("The rainbow is already solid.")
        else:
            print("The sceptre glows briefly.")
```

**Solution:**
```
> WAVE SCEPTRE
Suddenly, the rainbow appears to become solid!

> CROSS RAINBOW
End of Rainbow
You are on a small rocky outcropping. A solid rainbow stretches west.
There is a pot of gold here!

> TAKE POT
Taken.
```

#### 2. Rope Climbing

From [1actions.zil:3050](1actions.zil#L3050):

```python
class ROPE(Object):
    """Rope for climbing down from dome"""
    location = DOME_ROOM
    synonyms = ["ROPE"]
    adjectives = ["HEAVY"]
    description = "heavy rope"
    flags = [TAKEBIT]
    action = rope_fcn

class RAILING(Object):
    """Railing in dome room"""
    location = DOME_ROOM
    synonyms = ["RAILING", "RAIL"]
    description = "metal railing"
    flags = [NDESCBIT]

# Rope tied state
DOME_FLAG = False

def rope_fcn():
    """Handle rope tying and climbing"""

    if current_verb == "TIE":
        if PRSI == RAILING and HERE == DOME_ROOM:
            global DOME_FLAG
            DOME_FLAG = True
            ROPE.location = DOME_ROOM
            ROPE.add_flag(NDESCBIT)
            print("The rope is tied to the railing.")
        else:
            print("To what?")

    elif current_verb == "UNTIE":
        if DOME_FLAG:
            DOME_FLAG = False
            ROPE.remove_flag(NDESCBIT)
            print("The rope is untied.")
        else:
            print("The rope isn't tied.")

    elif current_verb == "CLIMB":
        if DOME_FLAG and HERE == DOME_ROOM:
            print("You climb down the rope.")
            move_player(TORCH_ROOM)
        elif DOME_FLAG and HERE == TORCH_ROOM:
            print("You climb up the rope.")
            move_player(DOME_ROOM)
        else:
            print("You can't climb that here.")

# Room descriptions check DOME_FLAG
class DOME_ROOM(Room):
    def long_desc(self):
        if DOME_FLAG:
            return "You are in the Dome Room. A heavy rope is tied to the railing and dangles down into darkness."
        else:
            return "You are in the Dome Room. There is a metal railing here."
```

#### 3. Forest Navigation

```python
# Multiple interconnected forest rooms create maze effect
class FOREST_1(Room):
    description = "Forest"
    exits = {
        NORTH: FOREST_2,
        SOUTH: FOREST_3,
        EAST: FOREST_4,
        WEST: CLEARING
    }
    global_objects = [FOREST, TREE]

class FOREST_2(Room):
    description = "Forest"
    exits = {
        NORTH: FOREST_1,  # Loops!
        SOUTH: FOREST_1,
        EAST: FOREST_5,
        WEST: FOREST_1
    }
    global_objects = [FOREST, TREE]

# Strategy: Drop items to mark path
# > DROP SWORD
# > NORTH
# > WEST
# > SOUTH
# There is a sword here. (You've been here before!)
```

#### 4. Maze Navigation

```python
# 15-room maze with complex connections
class MAZE_1(Room):
    description = "Maze"
    long_desc = """
You are in a maze of twisty little passages, all alike.
    """
    exits = {
        NORTH: MAZE_4,
        SOUTH: MAZE_3,
        EAST: MAZE_5,
        WEST: MAZE_12
    }

# One-way passages (MAZE-DIODES)
class MAZE_5(Room):
    description = "Maze"
    exits = {
        NORTH: MAZE_1,  # Can go north...
        SOUTH: None,    # ...but not back south
        EAST: MAZE_7,
        WEST: MAZE_11
    }

    # Contains skeleton keys!
    children = [KEYS]

# Strategy: Drop items to map
# Create mental map of connections
# Find KEYS in MAZE-5
```

### F. Item Combination Puzzles

#### 1. Egg and Canary

From [1actions.zil:2970](1actions.zil#L2970):

**Wind canary in forest** to attract songbird and get bauble.

```python
class EGG(Object):
    """Jeweled egg - fragile!"""
    location = BIRDS_NEST
    synonyms = ["EGG"]
    adjectives = ["JEWEL", "JEWELED"]
    description = "jeweled egg"
    treasure_value = 5
    flags = [TAKEBIT, CONTBIT, TRANSBIT]

    def on_drop(self):
        """Egg breaks if dropped!"""
        print("The egg falls and breaks into a thousand pieces.")
        self.treasure_value = 0  # Worthless!

        # Canary also breaks
        if CANARY.location == self:
            CANARY.broken = True

class CANARY(Object):
    """Wind-up canary"""
    location = EGG
    synonyms = ["CANARY", "BIRD"]
    adjectives = ["CLOCKWORK", "WIND-UP"]
    description = "clockwork canary"
    treasure_value = 4
    flags = [TAKEBIT]
    action = canary_fcn

def canary_fcn():
    """Wind canary to attract songbird"""

    if current_verb == "WIND":
        if HERE in [FOREST_1, FOREST_2, FOREST_3]:
            print("""
The canary chirps merrily as you wind it. A large songbird flies down
from the trees, picks up the canary in its beak, and flies away. As it
departs, it drops a beautiful pearl bauble.
            """)

            # Canary removed
            CANARY.remove_from_game()

            # Bauble appears
            BAUBLE.location = HERE
        else:
            print("The canary chirps briefly and stops.")

class BAUBLE(Object):
    """Reward for canary puzzle"""
    synonyms = ["BAUBLE", "PEARL"]
    adjectives = ["PEARL"]
    description = "pearl bauble"
    treasure_value = 1
    flags = [TAKEBIT]
```

**Solution:**
```
> TAKE EGG
(carefully) Taken.

> OPEN EGG
Opening the egg reveals a clockwork canary.

> TAKE CANARY
Taken.

> GO TO FOREST
Forest

> WIND CANARY
The canary chirps merrily as you wind it. A large songbird flies down
from the trees, picks up the canary in its beak, and flies away. As it
departs, it drops a beautiful pearl bauble.

> TAKE BAUBLE
Taken.
```

#### 2. Beach Digging

From [1actions.zil:2855](1actions.zil#L2855):

**Dig three times** to reveal scarab - fourth dig kills you!

```python
class SHOVEL(Object):
    location = SAND_ROOM
    synonyms = ["SHOVEL", "SPADE"]
    description = "shovel"
    flags = [TAKEBIT, TOOLBIT]

# Dig counter
BEACH_DIG = 0

def sandy_beach_digging():
    """Progressive digging reveals scarab"""
    global BEACH_DIG

    if current_verb == "DIG" and HERE == SANDY_BEACH:
        if SHOVEL not in inventory():
            print("Digging with your hands is slow and tedious.")
            return

        BEACH_DIG += 1

        if BEACH_DIG == 1:
            print("You seem to be digging a hole here.")
        elif BEACH_DIG == 2:
            print("The hole is getting deeper, but that's about it.")
        elif BEACH_DIG == 3:
            print("You are surrounded by a wall of sand on all sides. Digging still further, your shovel clanks against something metallic.")
            SCARAB.location = SANDY_BEACH
            SCARAB.remove_flag(INVISIBLE)
        elif BEACH_DIG >= 4:
            # Death!
            jigs_up("The hole collapses, smothering you.")

class SCARAB(Object):
    """Hidden treasure at beach"""
    location = SANDY_BEACH
    synonyms = ["SCARAB"]
    adjectives = ["CLAY", "EGYPTIAN"]
    description = "clay scarab"
    treasure_value = 5
    flags = [TAKEBIT, INVISIBLE]
```

**Solution:**
```
> DIG
You seem to be digging a hole here.

> DIG
The hole is getting deeper, but that's about it.

> DIG
You are surrounded by a wall of sand on all sides. Digging still further,
your shovel clanks against something metallic.
There is a clay scarab here!

> TAKE SCARAB
Taken.

> DIG
The hole collapses, smothering you.
****  You have died  ****
```

#### 3. Buoy and Emerald

From [1actions.zil:2844](1actions.zil#L2844):

**Hidden emerald** inside buoy on river.

```python
class BUOY(Object):
    """Red buoy with hidden emerald"""
    location = FRIGID_RIVER_1
    synonyms = ["BUOY"]
    adjectives = ["RED"]
    description = "red buoy"
    flags = [TAKEBIT, CONTBIT]

    first_desc = """
A red buoy bobs in the water here. There is something funny about
the feel of it.
    """

    def on_examine(self):
        """Hints at hidden contents"""
        print("The buoy feels peculiarly heavy.")

class EMERALD(Object):
    """Hidden inside buoy"""
    location = BUOY
    synonyms = ["EMERALD", "GEM"]
    adjectives = ["LARGE"]
    description = "large emerald"
    treasure_value = 10
    flags = [TAKEBIT]

# Solution: OPEN BUOY or LOOK IN BUOY
# > OPEN BUOY
# Opening the buoy reveals a large emerald.
```

### G. Word/Spell Puzzles

#### 1. Magic Words

```python
# Various spell words recognized
SPELL_WORDS = [
    "FEEBLE",    # Zork III spell
    "FUMBLE",    # Zork III spell
    "FEAR",      # Limited effect
    "FREEZE",    # No effect in Zork I
    "FALL",      # No effect in Zork I
    "FERMENT",   # No effect in Zork I
    "FIERCE",    # No effect in Zork I
    "FENCE",     # No effect in Zork I
    "FANTASIZE"  # No effect in Zork I
]

# Most spells don't work in Zork I
def v_say():
    """Say command for magic words"""
    word = P_WORD

    if word in SPELL_WORDS:
        print("A hollow voice says \"Fool.\"")
    else:
        print(f'The echo responds: "{word} {word} {word}..."')
```

#### 2. ODYSSEUS Command

```python
def v_odysseus():
    """Special verb for cyclops puzzle"""
    if HERE == CYCLOPS_ROOM:
        cyclops_flees()
    else:
        print("That name has no power here.")
```

#### 3. ECHO Command

From [gverbs.zil:526](gverbs.zil#L526):

```python
def v_echo():
    """Easter egg command - echoes last word"""
    last_word = P_LAST_WORD

    if last_word:
        print(f'The echo responds: "{last_word} {last_word} {last_word}..."')
    else:
        print("The acoustics here are terrible.")
```

### H. Timing Puzzles

#### 1. Cyclops Countdown

```python
# Cyclops attacks after eating hot lunch
CYCLOWRATH = 0  # Turns until attack

def i_cyclops():
    """Countdown to cyclops attack"""
    global CYCLOWRATH

    if CYCLOWRATH > 0:
        CYCLOWRATH -= 1

        if CYCLOWRATH == 1:
            print("The cyclops appears very angry!")
        elif CYCLOWRATH == 0:
            print("The cyclops, enraged, attacks!")
            start_combat(CYCLOPS)

    return 1  # Continue interrupt

# Give water before countdown ends!
```

#### 2. Match Duration (2 turns)

```python
def i_match():
    """Match burns for only 2 turns"""
    turn_count += 1

    if turn_count >= 2:
        MATCHES.remove_flag(ONBIT)
        print("The match has gone out.")
        return -1  # Stop interrupt

    return 1
```

#### 3. Lamp Battery (200 turns)

```python
# Warnings at specific intervals
def i_lantern():
    global LAMP_TURNS

    if LAMP.has_flag(ONBIT):
        LAMP_TURNS -= 1

        if LAMP_TURNS == 100:
            print("The lamp appears a bit dimmer.")
        elif LAMP_TURNS == 70:
            print("The lamp is definitely dimmer now.")
        elif LAMP_TURNS == 15:
            print("The lamp is nearly out!")
        elif LAMP_TURNS == 0:
            LAMP.add_flag(RMUNGBIT)
            LAMP.remove_flag(ONBIT)
            print("The lamp has run out of power.")
            return -1

    return 1
```

### I. Container/Inventory Puzzles

#### 1. Trophy Case (Scoring Mechanism)

From [1actions.zil:442](1actions.zil#L442):

```python
class TROPHY_CASE(Object):
    """Trophy case - central scoring mechanism"""
    location = LIVING_ROOM
    synonyms = ["CASE", "TROPHY"]
    adjectives = ["TROPHY"]
    description = "trophy case"
    flags = [TRANSBIT, CONTBIT, OPENBIT, NDESCBIT]
    capacity = 10000

    def on_object_inserted(self, obj):
        """Score when treasure placed inside"""
        if obj.treasure_value > 0:
            SCORE += obj.treasure_value
            print(f"Your score has just gone up by {obj.treasure_value} points.")
            check_win_condition()
```

#### 2. Coffin (Container and Treasure)

```python
class COFFIN(Object):
    """Coffin is both container AND treasure"""
    location = EGYPT_ROOM
    synonyms = ["COFFIN", "CASKET"]
    adjectives = ["GOLD"]
    description = "gold coffin"
    treasure_value = 15  # Worth points itself!
    flags = [TAKEBIT, CONTBIT, OPENBIT]
    capacity = 20

    # Contains sceptre
    children = [SCEPTRE]

# Both coffin AND sceptre score points!
# > TAKE COFFIN
# Taken.
# > PUT COFFIN IN TROPHY CASE
# Your score has just gone up by 15 points.
# > PUT SCEPTRE IN TROPHY CASE
# Your score has just gone up by 6 points.
```

#### 3. Bag of Coins (Stupid Container)

```python
class BAG_OF_COINS(Object):
    """Bag you can't open - stupid container"""
    location = MAZE_5
    synonyms = ["BAG", "COINS"]
    adjectives = ["LEATHER"]
    description = "leather bag of coins"
    treasure_value = 10
    flags = [TAKEBIT, CONTBIT]  # Container but can't open!

    def on_open(self):
        """Cannot open bag"""
        print("The bag is tied shut and cannot be opened.")
        return False

    def on_examine(self):
        """Can hear coins inside"""
        print("The bag is full of coins. You can hear them clink.")

# Just a treasure - don't need to open it!
```

### J. Temple/Religious Puzzles

#### 1. Bell, Book, and Candle Exorcism

**(Already documented in NPC section - ghosts)**

Full ritual:
1. Ring brass bell
2. Read black book (open to page 569)
3. Light pair of candles
4. Perform in presence of evil spirits

#### 2. Prayer/Resurrection

```python
def v_pray():
    """PRAY verb - resurrection mechanic"""

    if HERE == SOUTH_TEMPLE and DEAD:
        # Resurrect!
        DEAD = False
        ALWAYS_LIT = False
        WINNER.strength = 0

        print("""
From the distance the sound of a lone trumpet is heard. The room becomes
very bright and you feel disembodied. In a moment, the feeling passes.
You are whole once again. You feel as if a great weight has been lifted
from your soul and you are free to go on with your life.
        """)

        move_player(FOREST_1)

    elif HERE == SOUTH_TEMPLE:
        print("Your prayers are heard, and a sense of peace fills the room.")
    else:
        print("Your prayers don't seem to be heard.")
```

#### 3. Sacred Objects

```python
# Bodies in temple - desecration = death
class SKELETON(Object):
    location = TEMPLE
    synonyms = ["SKELETON", "BONES"]
    description = "skeleton"
    flags = [NDESCBIT]

    def on_take(self):
        """Disturbing skeleton curses you"""
        jigs_up("""
The ghost of the skeleton appears and curses you for disturbing his rest!
        """)

class BODIES(Object):
    location = SOUTH_TEMPLE
    synonyms = ["BODY", "BODIES", "CORPSE"]
    description = "ancient bodies"
    flags = [NDESCBIT]

    def on_desecrate(self):
        """Disrespecting bodies = divine punishment"""
        jigs_up("The gods do not take kindly to such sacrilege!")
```

---

## 6. Special Mechanics

### A. Thief Behavior System

From [1actions.zil:1724](1actions.zil#L1724):

**Random appearances, stealing, combat**.

```python
# Thief state
THIEF_ENGROSSED = False  # Distracted by loot
ROBBER_STATE = 0         # Tracking thief behavior

def i_thief():
    """Interrupt: Thief random appearances"""

    if random(100) < 30:  # 30% chance per turn
        # Thief appears
        THIEF.location = random_room()

        if THIEF.location == HERE:
            print("A seedy-looking individual appears from the shadows!")

        # Attempt theft
        attempt_theft()

        # Disappear after 2-3 turns
        schedule_disappearance()

def attempt_theft():
    """Thief steals valuable objects"""
    # Prioritize treasures
    targets = [obj for obj in HERE.children + inventory()
               if obj.treasure_value > 0]

    # Or take anything valuable
    if not targets:
        targets = [obj for obj in inventory()
                   if obj.has_flag(TAKEBIT)]

    if targets and random(100) < 70:  # 70% success
        stolen = random_choice(targets)
        stolen.location = THIEF
        stolen.add_flag(INVISIBLE)  # Hidden until thief defeated

        print(f"The thief steals the {stolen.desc}!")

        # Thief becomes engrossed (distracted)
        THIEF_ENGROSSED = True

    # Flee
    THIEF.location = None

def defeat_thief():
    """Thief death - treasures appear in treasure room"""
    print("""
The thief is killed. His booty, and his large bag, are left behind.
    """)

    # All stolen items appear in TREASURE-ROOM
    for obj in THIEF.children:
        obj.location = TREASURE_ROOM
        obj.remove_flag(INVISIBLE)

    # Stiletto drops
    STILETTO.location = HERE

    THIEF.remove_from_game()
```

### B. Object Randomization After Death

From [1actions.zil:4101](1actions.zil#L4101):

```python
def randomize_objects():
    """Scatter items after player death"""

    # List of above-ground rooms
    surface_rooms = [
        WEST_OF_HOUSE,
        NORTH_OF_HOUSE,
        SOUTH_OF_HOUSE,
        EAST_OF_HOUSE,
        KITCHEN,
        LIVING_ROOM,
        ATTIC,
        CELLAR
    ]

    # Scatter inventory items
    for obj in inventory():
        if obj.has_flag(TAKEBIT):
            obj.location = random_choice(surface_rooms)

    # Lamp always returns to living room
    LAMP.location = LIVING_ROOM
    LAMP.remove_flag(ONBIT)
```

### C. Strength/Injury System

```python
# Player strength tracking
class ADVENTURER(Object):
    strength = 0  # 0 = healthy, negative = injured

    def apply_wound(self, damage):
        """Take injury"""
        self.strength -= damage

        # Reduce carrying capacity
        global LOAD_ALLOWED
        LOAD_ALLOWED = 100 + (self.strength * 10)

        if self.strength <= -5:
            jigs_up("You have died from your injuries.")

def i_cure():
    """Gradual healing over time"""
    if WINNER.strength < 0:
        WINNER.strength += 1

        if WINNER.strength == 0:
            print("You feel much better now.")
            LOAD_ALLOWED = 100

    return CURE_WAIT  # Continue healing
```

### D. Global State Flags

```python
# Major puzzle state tracking
TROLL_FLAG = False      # Troll defeated - passages open
CYCLOPS_FLAG = False    # Cyclops asleep/defeated
MAGIC_FLAG = False      # Cyclops wall opened
GATES_OPEN = False      # Dam gates state
LOW_TIDE = True         # Reservoir water level
RAINBOW_FLAG = False    # Rainbow solid/crossable
DOME_FLAG = False       # Rope tied in dome
WON_FLAG = False        # Game completed
DEAD = False            # Player is spirit
GRUNLOCK = False        # Grate unlocked

# Thief tracking
ROBBER_STATE = 0
THIEF_ENGROSSED = False

# Cyclops tracking
CYCLOWRATH = 0  # Turns until attack

# Death tracking
DEATHS = 0  # Number of deaths

# Digging tracking
BEACH_DIG = 0  # Beach dig count

# Item tracking
MATCH_COUNT = 20        # Matches remaining
LAMP_TURNS = 200        # Lamp battery remaining
CANDLE_TURNS = 40       # Candle burn time
```

### E. Key Object Flags

```python
# Flag meanings (from gdefs.zil)
TAKEBIT =      1  << 0   # Can be taken
CONTBIT =      1  << 1   # Is a container
OPENBIT =      1  << 2   # Container/door is open
TOUCHBIT =     1  << 3   # Has been touched/interacted with
ONBIT =        1  << 4   # Light source is lit
DOORBIT =      1  << 5   # Is a door
TRANSBIT =     1  << 6   # Transparent (can see inside)
FOODBIT =      1  << 7   # Is food
NDESCBIT =     1  << 8   # No description in room listing
INVISIBLE =    1  << 9   # Cannot be seen
ACTORBIT =     1  << 10  # Is an NPC/actor
FIGHTBIT =     1  << 11  # In combat mode
WEAPONBIT =    1  << 12  # Can be used as weapon
BURNBIT =      1  << 13  # Can burn/be destroyed by fire
FLAMEBIT =     1  << 14  # Is a flame/fire source
TOOLBIT =      1  << 15  # Can be used as tool (UNLOCK, etc.)
LIGHTBIT =     1  << 16  # Is a light source
VEHBIT =       1  << 17  # Is a vehicle
SURFACEBIT =   1  << 18  # Surface (not container)
READBIT =      1  << 19  # Can be read
RMUNGBIT =     1  << 20  # Destroyed/used up
CLIMBBIT =     1  << 21  # Can be climbed
SACREDBIT =    1  << 22  # Sacred/protected
TRYTAKEBIT =   1  << 23  # Looks takeable but isn't
WEARBIT =      1  << 24  # Can be worn
```

---

## Summary

### Puzzle Complexity Breakdown

| Category | Simple | Medium | Complex |
|----------|--------|--------|---------|
| **Lock & Key** | Front door | Trap door | Grate |
| **Light** | Matches | Candles | Lamp battery |
| **Water** | Bottle | Boat | Dam system |
| **NPC** | - | Troll | Thief, Cyclops |
| **Environment** | Rope | Forest | Maze, Rainbow |
| **Items** | Buoy | Beach dig | Egg/canary |
| **Combat** | - | Troll | Thief, Cyclops |

### Critical Path Items

To complete Zork I, you **must**:
1. Get brass lantern (light for underground)
2. Find skeleton keys (unlock grate)
3. Collect 19 treasures (350 points)
4. Place all treasures in trophy case
5. Take map that appears
6. Enter Stone Barrow

### Death Avoidance Strategy

**Most dangerous:**
- Darkness (grue) - always carry light
- Combat - avoid or prepare well
- Cursed items - don't use rusty knife
- Rainbow - wave sceptre first
- Beach digging - stop at 3 digs

**Safest strategy:**
- Carry lamp everywhere
- Avoid combat when possible
- Read item descriptions carefully
- Save often (if playing on emulator)
- Use PRAY in temple if you die

---

This comprehensive guide covers all puzzle mechanics, combat, death conditions, and the path to victory in Zork I!
