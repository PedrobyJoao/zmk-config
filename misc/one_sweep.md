## 1. Design Philosophy & Strategy

To meet these requirements, a multi-faceted strategy
was adopted, centered around layers and advanced ZMK
behaviors.

### A. Home Row Mods

The core of this layout is the "Home Row Mods"
concept. Instead of dedicating keys on the bottom row
or thumb cluster to all modifiers, `Shift`, `Alt`, and
`Ctrl` are placed on home row keys.

- **Mechanism:** This is achieved with a custom
  `hold-tap` behavior (`hm`). Tapping a key produces
  a letter (e.g., `E`), while holding it produces a
  modifier (e.g., `Alt`).
- **Rationale:** This drastically reduces hand movement,
  keeping fingers on or near the home position as much
  as possible. It's highly efficient for chording once
  learned. The `flavor` is set to `tap-preferred` to
  prevent accidental modifier activation during fast
  typing.

### B. Four-Layer System

A four-layer system provides a clean separation of
concerns, preventing any single layer from becoming
too cluttered.

1.  **DEFAULT (Layer 0):** For standard alpha-numeric
    typing and home row modifiers.
2.  **SYM (Layer 1):** For numbers and programming symbols.
3.  **NAV (Layer 2):** For navigation, media controls, and a numpad.
4.  **FUNC (Layer 3):** For F-keys and Bluetooth controls.

### C. Thumb Cluster Prioritization

The six thumb keys are the most valuable real estate.
They are dedicated to the highest priority actions that
don't fit on the home row.

- **Dedicated Keys:** `Backspace`, `Enter`, and `Space`
  are given their own dedicated keys to ensure they are
  holdable and reliable.
- **Holdable Super:** `Super` (`LGUI`) is also on a
  dedicated key, making it a pure, holdable modifier.
- **Layer Access:** The remaining two thumb keys are
  used for layer switching (`NAV` and `SYM`), providing
  quick and ergonomic access to the most frequently
  used layers.

## 2. Layer Breakdown

### Layer 0: DEFAULT

- **Layout:** Standard QWERTY.
- **Home Row Mods:**
  - `A` / `;` = `Shift`
  - `E` / `I` = `Alt`
  - `R` / `U` = `Ctrl`
- **Thumbs (Left to Right):**
  1. `NAV` (hold) / `TAB` (tap)
  2. `Backspace` (dedicated)
  3. `Super` (dedicated, holdable)
  4. `SYM` (hold)
  5. `Enter` (dedicated)
  6. `Space` (dedicated)

### Layer 1: SYM (Symbol Layer)

- **Philosophy:** "Shift for Symbols." This layer
  primarily contains numbers and base symbols. Shifted
  symbols (e.g., `!`, `@`, `+`, `_`) are produced by
  holding `Shift` (via a home row mod) and pressing
  the corresponding number or symbol key. This
  leverages existing muscle memory from standard
  keyboards.
- **Layout:**
  - **Top Row:** `N1` through `N0`.
  - **Middle/Bottom Rows:** Carefully arranged symbols
    like `[]`, `()`, `-`, `=`, `\`, `'`, `"` to feel
    intuitive.
- **Workflow Enhancement:** The thumb key positions for
  `Backspace` and `Enter` are mapped to `&trans`. This
  allows the user to press them without leaving the
  `SYM` layer, creating a seamless workflow for tasks
  like calculations.

### Layer 2: NAV (Navigation Layer)

- **Purpose:** Centralizes movement and device controls.
- **Layout (Left Hand):**
  - `hjkl`-style arrow keys (`LEFT`, `DOWN`, `UP`, `RIGHT`).
  - `Home`/`End`, `PgUp`/`PgDn`.
  - `ESC` on the top-left corner.
  - Brightness controls.
- **Layout (Right Hand):**
  - A full numpad layout (`KP_N0` - `KP_N9`, `KP_DOT`,
    etc.) for dedicated numeric entry.
- **FUNC Layer Access:** The `FUNC` layer is accessed
  by holding a thumb key while in the `NAV` layer, a
  comfortable two-thumb combination.

### Layer 3: FUNC (Function Layer)

- **Purpose:** Houses less frequently used but still
  important keys.
- **Layout:**
  - `F1` through `F12`.
  - Bluetooth controls (`BT_SEL`, `BT_CLR`).

## 3. Custom Behaviors

To achieve the desired functionality, two custom
`hold-tap` behaviors were created.

- **`hm: homerow_mods`**:

  - **Purpose:** The engine for all home row mods.
  - **Key Setting:** `flavor = "tap-preferred"`. This
    is critical. It prioritizes the tap action
    (sending a letter), preventing misfires when typing
    quickly. The hold action (modifier) only triggers
    on a deliberate hold.

- **`lt_nav_tab: layer_tap_nav_tab`**:
  - **Purpose:** Governs the behavior of the `NAV`/`TAB`
    thumb key.
  - **Key Setting:** `flavor = "hold-preferred"`. This
    prioritizes the hold action (activating the layer),
    making layer switching feel more responsive. It is
    defined separately to allow its timings to be tuned
    independently of other `hold-tap` keys.

## 4. Combos

- **`combo_bootloader` & `combo_reset`**:
  - **Requirement:** These destructive actions must be
    very difficult to trigger by accident.
  - **Design:** They are implemented as combos requiring
    all six thumb keys to be held down simultaneously
    plus a specific key on the home row (`Q` for
    bootloader, `R` for reset). This makes accidental
    activation virtually impossible.
