# ZORK ACTION SYSTEM GLOSSARY

This glossary documents the key aliases and variables used in Zork I's action and parser system.

## Core Action Variables

| Variable | Location | Description |
|----------|----------|-------------|
| **PRSA** | gparser.zil:14 | **PR**imary **S**yntax/**A**ction - The current verb/action being performed (e.g., V?TAKE, V?DROP) |
| **PRSO** | gparser.zil:16 | **PR**imary **S**yntax/**O**bject - The direct object of the action (e.g., "lamp" in "TAKE LAMP") |
| **PRSI** | gparser.zil:15 | **PR**imary **S**yntax/**I**ndirect object - The indirect object (e.g., "box" in "PUT LAMP IN BOX") |
| **WINNER** | gparser.zil:32 | The current actor performing the action (usually ADVENTURER, but changes when commanding NPCs) |
| **PLAYER** | gmain.zil:7 | Reference to the original player character (constant reference to ADVENTURER) |

## Location & Environment Variables

| Variable | Location | Description |
|----------|----------|-------------|
| **HERE** | gparser.zil:31 | Current room/location where WINNER is standing |
| **LIT** | gverbs.zil:1626 | Boolean indicating whether the current room is lit (affects visibility) |

## Parser Tables

| Variable | Location | Description |
|----------|----------|-------------|
| **P-PRSO** | gparser.zil:964 | Parser table holding all matched direct objects (when using ALL or multiple objects) |
| **P-PRSI** | gparser.zil:965 | Parser table holding all matched indirect objects |
| **P-ITBL** | gparser.zil:83 | Parser **I**nput **T**a**BL**e - Stores parsed command components (verb, prepositions, noun clauses) |
| **P-IT-OBJECT** | gparser.zil:59 | The object referenced by pronouns (IT, THEM, HIM, HER) |
| **P-LEXV** | gparser.zil:34 | Parser **LEX**ical **V**ector - Tokenized input from current command |

## Parser Constants (P-ITBL Indices)

| Constant | Location | Description |
|----------|----------|-------------|
| **P-NC1** | gparser.zil:96 | **N**oun **C**lause **1** index - First noun phrase in P-ITBL |
| **P-NC1L** | gparser.zil:97 | Noun Clause 1 **L**ength |
| **P-NC2** | gparser.zil:98 | **N**oun **C**lause **2** index - Second noun phrase in P-ITBL |
| **P-NC2L** | gparser.zil:99 | Noun Clause 2 **L**ength |

## Parser State Flags

| Variable | Location | Description |
|----------|----------|-------------|
| **P-CONT** | gparser.zil:58 | Parser **CONT**inuation flag - Used for multi-turn commands |
| **P-OFLAG** | gparser.zil:64 | **O**rphan **FLAG** - Indicates incomplete command needing more input |
| **P-MULT** | gmain.zil:174 | **MULT**iple objects flag - Set when action applies to multiple objects |
| **QUOTE-FLAG** | gparser.zil:101 | Indicates if parser is inside quoted text (for TELL commands) |
| **P-WON** | gmain.zil:9 | Parser **WON** - Result of parser (whether it successfully parsed input) |

## Action Dispatch Return Values

| Constant | Value | Location | Description |
|----------|-------|----------|-------------|
| **M-HANDLED** | 1 | gmain.zil:13 | Action was successfully handled, stop dispatch chain |
| **M-NOT-HANDLED** | <> (false) | gmain.zil:15 | Action not handled, continue to next handler in chain |
| **M-FATAL** | 2 | gmain.zil:11 | Fatal error, stop all processing |

## Room Action Messages

| Constant | Value | Location | Description |
|----------|-------|----------|-------------|
| **M-BEG** | 1 | gmain.zil:19 | Room **BEG**in action - Called before player command |
| **M-END** | 6 | gmain.zil:21 | Room **END** action - Called after player command |
| **M-ENTER** | 2 | gmain.zil:23 | Called when player enters room |
| **M-LOOK** | 3 | gmain.zil:25 | Called when player looks at room |
| **M-FLASH** | 4 | gmain.zil:27 | Quick room description (brief mode) |
| **M-OBJDESC** | 5 | gmain.zil:29 | Object description in room |

## Special Objects

| Object | Location | Description |
|----------|----------|-------------|
| **ADVENTURER** | gglobals.zil:267 | The player character object (has ACTORBIT flag) |
| **ME** | gglobals.zil:214 | Pseudo-object representing the player in commands ("EXAMINE ME") |
| **IT** | gglobals.zil:42 | Pronoun object (IT, THEM, HIM, HER) - resolved to actual object at runtime |
| **NOT-HERE-OBJECT** | gglobals.zil:48 | Special object used when parser can't find referenced object |

## Key Macros

| Macro | Location | Description |
|----------|----------|-------------|
| **VERB?** | gmacros.zil:52 | Check if PRSA matches any of the given verbs: `<VERB? TAKE DROP>` |
| **PRSO?** | gmacros.zil:55 | Check if PRSO matches any of the given objects |
| **PRSI?** | gmacros.zil:58 | Check if PRSI matches any of the given objects |
| **ROOM?** | gmacros.zil:61 | Check if HERE matches any of the given rooms |
| **TELL** | gmacros.zil:9 | Print formatted text: `<TELL "You see " D ,LAMP>` |

## Action Dispatch Flow (PERFORM routine)

**Location:** gmain.zil:262-284

When an action is performed, handlers are called in this order:

1. **WINNER's action** - The current actor's action handler
2. **Room M-BEG** - Current room's pre-action handler
3. **PREACTIONS** - Global pre-action table
4. **PRSI action** - Indirect object's action handler
5. **Container action** - PRSO's container's action handler
6. **PRSO action** - Direct object's action handler
7. **Default action** - Verb's default handler from ACTIONS table

Each handler can return:
- `M-HANDLED` - Stop the dispatch chain
- `M-NOT-HANDLED` - Continue to next handler
- `M-FATAL` - Stop all processing

## Common Usage Patterns (Python Pseudocode)

### Check verb and object
```python
# ZIL: <COND (<AND <VERB? TAKE> <PRSO? LAMP>>
#             <TELL "You take the lamp." CR>
#             <RTRUE>)>

if PRSA in [VERB_TAKE] and PRSO == LAMP:
    print("You take the lamp.")
    return True
```

### Check current location
```python
# ZIL: <COND (<ROOM? WEST-OF-HOUSE EAST-OF-HOUSE>
#             <TELL "You are near the house." CR>)>

if HERE in [WEST_OF_HOUSE, EAST_OF_HOUSE]:
    print("You are near the house.")
```

### Check object in winner's inventory
```python
# ZIL: <COND (<IN? ,LAMP ,WINNER>
#             <TELL "You are carrying the lamp." CR>)>

if LAMP.location == WINNER:
    print("You are carrying the lamp.")
```

### Handle multiple verbs
```python
# ZIL: <COND (<VERB? OPEN UNLOCK>
#             <TELL "The door opens." CR>
#             <FSET ,DOOR ,OPENBIT>
#             <RTRUE>)>

if PRSA in [VERB_OPEN, VERB_UNLOCK]:
    print("The door opens.")
    DOOR.flags.add(OPENBIT)
    return True
```

### Access global variables
```python
# ZIL: <SETG WINNER ,PLAYER>
# ZIL: <MOVE ,LAMP ,HERE>

WINNER = PLAYER
LAMP.location = HERE

# Period prefix for local variable access (in function arguments)
# ZIL: <TELL "You see " D .OBJ>
print(f"You see {obj.description}")
```

## Notes

- **Comma (,) prefix**: Accesses global variables/objects (e.g., `,WINNER`, `,LAMP`)
- **Period (.) prefix**: Accesses local variables (e.g., `.OBJ`, `.RARG`)
- **WINNER vs PLAYER**: WINNER changes when commanding NPCs; PLAYER always refers to the adventurer
- **Action handlers**: Return M-HANDLED to stop dispatch, M-NOT-HANDLED to continue
- **Room messages**: M-BEG, M-ENTER, M-LOOK are called with room action routines

---

This glossary provides the foundation for understanding how Zork's sophisticated parser and action system work together to create interactive fiction.
