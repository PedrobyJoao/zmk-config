# Swweeep Keyboard Keymap Documentation

This document outlines the requirements, design
philosophy, and implementation details for a custom ZMK
keymap for the Swweeep keyboard. The layout is
tailored for a software engineer who prioritizes
ergonomic access to programming symbols and modifiers
without sacrificing typing speed.

## 1. Core Requirements

The primary goal was to create a highly efficient
layout on a 36-key split keyboard. The following
requirements were established:

### Priority 1 Keys (Must be easily accessible)

- `Super` (GUI/Win/Cmd)
- `Backspace`
- `Enter`
- `Space`
- `Tab`
- `Shift`

### Priority 2 Keys (Frequent use)

- `Ctrl`
- `Alt`
- Programming Symbols: `=`, `-`, `_`, `[`, `]`, `{`, `}`,
  `|`, `\`, `?`, `~`, `` ` ``, `;`, `:`, `"`, `'`

### Critical Key Combinations

The layout must make the following combinations
comfortable to execute:

- `super + shift`
- `super + hjkl` (for window management)
- `alt + nums`
- `super + nums`
- `alt + hjkl`
- `ctrl + v`

### Ergonomic Constraints

- **Holdable Keys:** `Backspace` and `Enter` must be
  holdable to repeat their actions (e.g., deleting
  multiple characters). This constraint invalidates
  placing them on the tap action of a layer-tap key.
- **Symbol Placement:** The layout for symbols should,
  as much as possible, respect the "geographical"
  muscle memory from standard keyboards to reduce the
  learning curve.
- **Calculation Workflow:** Performing calculations
  (typing numbers and operators) should be fluid, with
  easy access to `Backspace` and `Enter` without
  leaving the number layer.
