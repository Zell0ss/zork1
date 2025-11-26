# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains the source code for **Zork I: The Great Underground Empire** (1980), a classic interactive fiction game by Infocom. The code is written in **ZIL (Zork Implementation Language)**, a dialect of LISP/MDL created for text adventure game development.

**Important Context:**
- This is historical source code from Infocom's final shutdown
- There is **no official Infocom compiler** available to build this code
- The user-maintained [ZILF compiler](http://zilf.io) can compile these .ZIL files with minor modifications
- A pre-compiled Z-machine file exists at `COMPILED/zork1.z3`

## File Structure and Architecture

### Main Entry Point
- **zork1.zil** - Root file that includes all other modules in dependency order:
  1. GMACROS - Macro definitions
  2. GSYNTAX - Syntax definitions for parser
  3. 1DUNGEON - Game world objects and room definitions
  4. GGLOBALS - Global variables
  5. GCLOCK - Timer/daemon system
  6. GMAIN - Main game loop and PERFORM dispatch
  7. GPARSER - Natural language parser
  8. GVERBS - Verb action handlers
  9. 1ACTIONS - Game-specific action routines and room behaviors

### Core Modules

**gmain.zil** - Game execution engine
- `MAIN-LOOP` and `MAIN-LOOP-1`: Central game loop handling player input
- `PERFORM`: Action dispatch system that routes commands through this hierarchy:
  1. WINNER's action handler (player/NPC)
  2. Current room's M-BEG action
  3. PREACTIONS table
  4. PRSI (indirect object) action
  5. PRSO's container action
  6. PRSO (direct object) action
  7. Default action handler

**gparser.zil** - Natural language parser
- Tokenizes player input into commands
- Handles pronouns (IT), ALL, multiple objects
- Manages lexical analysis (P-LEXV tables)
- Supports OOPS for typo correction

**1dungeon.zil** - World definition
- OBJECT definitions with properties: SYNONYM, DESC, FLAGS, ACTION
- Room definitions with directional exits
- Global vs. local object scoping

**1actions.zil** - Action implementations
- Room-specific behaviors (WEST-HOUSE, EAST-HOUSE, etc.)
- Object action routines (BOARD-F, TEETH-F, etc.)
- Puzzle logic and game mechanics

**gverbs.zil** - Verb handlers
- Standard adventure game verbs (TAKE, DROP, EXAMINE, etc.)
- Default implementations for common actions

## ZIL Language Basics

**Syntax**: LISP-like with angle brackets
```zil
<ROUTINE NAME (ARGS) body>
<OBJECT NAME (properties...)>
<GLOBAL VAR-NAME initial-value>
```

**Common Constructs**:
- `<TELL "text">` - Print text
- `<COND (test1 action1) (test2 action2)>` - Conditional
- `<VERB? TAKE EXAMINE>` - Check if current verb matches
- `<FSET? obj FLAG>` / `<FCLEAR obj FLAG>` - Object flag manipulation
- `,VARIABLE` - Global variable access (comma prefix)
- `.VARIABLE` - Local variable access (period prefix)

## Key Global Variables

- `PRSA` - Current verb/action
- `PRSO` - Direct object (primary object)
- `PRSI` - Indirect object
- `WINNER` - Current actor (usually the player)
- `HERE` - Current location
- `LIT` - Whether current room is lit

## Return Values for Action Handlers

- `M-HANDLED` (1) - Action successfully handled
- `M-NOT-HANDLED` (<>) - Action not handled, continue dispatch
- `M-FATAL` (2) - Fatal error, stop processing

## Compilation Notes

Since no official compiler exists:
- This source code is primarily for **historical study and research**
- To create a playable game, use the ZILF compiler (external tool)
- The pre-compiled `COMPILED/zork1.z3` can be played with any Z-machine interpreter

## Commands

There are no build, test, or lint commands for this repository as it contains historical source code without modern development tooling.

To study the code:
- Read .ZIL files directly
- Start with `zork1.zil` to understand module dependencies
- Use the pre-compiled Z-machine file to understand runtime behavior
