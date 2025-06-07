/web https://zmk.dev/docs/keymaps/behaviors https://zmk.dev/docs/keymaps

I need you to help to create my swweeep keymap based on my necessities.

But my necessities are not very clear yet so you have to discover
them by asking.

I'm a software engineer btw

Some of them are:

- QWERTY first layer (already there)
- Priotity 1 keys:
  - Super
  - Backspace
  - Enter
  - Space
  - Tab
  - Shift
- Priority 2 keys:
  - Ctrl
  - Alt
  - `=`, `-`, `{` and other programming symbols
- Blend of keys that I most use:
  - super + shift
  - super + hjkl
  - alt + nums
  - super + nums
  - alt + hjkl
  - ctrl + v

What should be on the same position as a normal keyboard:

1. nums can be numpad or in a row
2. the symbols sharing the same positions as nums on a normal  
   keyboard MUST be the first top row on our setup here (!, @...)) as  
   it's already configured on the default keymap
3. - _ ~ ` ; " ' [ ] { } | \ ? are also important and I would  
     like to position then more or less on the same way as they are  
     already postioned on a normal keyboard. Example of the top  
     row on the right would be: -_ =+ second row: [{ ]} \|

Other notes:

- the layer which contains numbers should still keep backspace,  
  dot, comma and enter (but I think this can be achieved by using  
  $trans which will use the next layer when pressed)

ZMK docs:

````md
---
title: List of Keycodes
sidebar_label: List of Keycodes
---

import OsLegend from "@site/src/components/codes/OsLegend";
import ToastyContainer from "@site/src/components/codes/ToastyContainer";
import Table from "@site/src/components/codes/Table";

This is the reference page for keycodes used by behaviors. Use the table of contents (on the right or the top) for easy navigation.

:::warning
Take extra notice of the spelling of the keycodes, especially the shorthand spelling.
Otherwise, it will result in an elusive parsing error!
:::

:::info[Keyboard vs. Consumer keycodes]
In the below tables, there are keycode pairs with similar names where one variant has a `K_` prefix and another `C_`.
These variants correspond to similarly named usages from different [HID usage pages](https://usb.org/sites/default/files/hut1_2.pdf#page=16),
namely the "keyboard/keypad" and "consumer" ones respectively.

In practice, some OS and applications might listen to only one of the variants.
You can use the values in the compatibility columns below to assist you in selecting which one to use.
:::

<OsLegend />
<ToastyContainer />

## Keyboard

### Letters

<Table group="keyboard-letters" />

### Numbers

<Table group="keyboard-numbers" />

### Symbols / Punctuation

<Table group="keyboard-symbols" />

### Control & Whitespace

<Table group="keyboard-control-whitespace" />

### Navigation

<Table group="keyboard-navigation" />

### Locks

<Table group="keyboard-locks" />

### F Keys

<Table group="keyboard-fkeys" />

### International

<Table group="keyboard-international" />

### Language

<Table group="keyboard-language" />

### Miscellaneous

<Table group="keyboard-miscellaneous" />

## Modifiers

The [Modifiers](modifiers.mdx) page includes further information.

<Table group="keyboard-modifiers" />

## Keypad

<Table group="keypad" />

### Numbers

<Table group="keypad-numbers" />

### Symbols / Operations

<Table group="keypad-operations" />

## Editing

### Cut, Copy, Paste

<Table group="cut-copy-paste" />

### Undo, Redo

<Table group="undo-redo" />

## Media

### Sound / Volume

<Table group="sound" />

### Display

<Table group="display" />

### Media Controls

<Table group="media-controls" />

### Consumer Menus

<Table group="consumer-menus" />

### Consumer Controls

<Table group="consumer-controls" />

## Applications

### Application Controls

<Table group="application-controls" />

### Applications (Launch)

<Table group="applications" />

## Input Assist

<Table group="input-assist" />

## Power & Lock

## <Table group="power" />

title: Modifiers
sidebar_label: Modifiers

---

Modifiers are the special keyboard keys: _shift_, _alt_, _control_ & _GUI_. Their keycodes can be found in the [list of keycodes](./list-of-keycodes.mdx#modifiers).
Modifiers can be used both as [keys](#modifier-keys) and as [functions](#modifier-functions).

### Modifier Keys

These act like any other keycode.

- e.g. `&kp LEFT_GUI` pushes and releases the left GUI key.

### Modifier Functions

Modifier functions add one or more modifiers to a code.

These functions take the form: `XX(code)`

- Modifier functions apply a modifier to a code:
  - `&kp LS(A)` = `LEFT_SHIFT`+`A` (a capitalized **A**).
- They can be combined:
  - `&kp LC(RA(B))` = `LEFT_CONTROL`+`RIGHT_ALT`+`B`
- They can be applied to a modifier keycode to create combined modifier keys:
  - `&kp LS(LALT)` = `LEFT_SHIFT` + `LEFT_ALT`
- Some basic keycodes already include a modifier function in their definition:
  - `DOLLAR` = `LS(NUMBER_4)`
- There are left- and right-handed versions of each modifier (also see [table in the list of keycodes](./list-of-keycodes.mdx#modifiers)):
  - `LS(x)`, `LC(x)`, `LA(x)`, `LG(x)`, `RS(x)`, `RC(x)`, `RA(x)`, `RG(x)`
- Modified keys can safely be rolled-over. Modifier functions are released when another key is pressed.
  - Press `&kp LS(A)`, then press `&kp B`, release `&kp LS(A)` and release `&kp B` results in **Ab**. Only the A is capitalized.

---

## title: Combos

## Summary

Combo keys are a way to combine multiple keypresses to output a different key. For example, you can hit the Q and W keys on your keyboard to output escape.

### Configuration

Combos configured in your `.keymap` file, but are separate from the `keymap` node found there, since they are processed before the normal keymap. They are specified like this:

```dts
/ {
    combos {
        compatible = "zmk,combos";
        combo_esc {
            timeout-ms = <50>;
            key-positions = <0 1>;
            bindings = <&kp ESC>;
        };
    };
};
```
````

- The name of the combo doesn't really matter, but convention is to start the node name with `combo_`.
- The `compatible` property should always be `"zmk,combos"` for combos.
- All the keys in `key-positions` must be pressed within `timeout-ms` milliseconds to trigger the combo.
- `key-positions` is an array of key positions. See the info section below about how to figure out the positions on your board.
- `layers = <0 1...>` will allow limiting a combo to specific layers. This is an _optional_ parameter, when omitted it defaults to global scope.
- `bindings` is the behavior that is activated when the behavior is pressed.
- (advanced) you can specify `slow-release` if you want the combo binding to be released when all key-positions are released. The default is to release the combo as soon as any of the keys in the combo is released.
- (advanced) you can specify a `require-prior-idle-ms` value much like for [hold-taps](behaviors/hold-tap.mdx#require-prior-idle-ms). If any non-modifier key is pressed within `require-prior-idle-ms` before a key in the combo, the combo will not trigger.

:::info

Key positions are numbered like the keys in your keymap, starting at 0. So, if the first key in your keymap is `Q`, this key is in position `0`. The next key (possibly `W`) will have position 1, etcetera.

:::

### Advanced Usage

- Partially overlapping combos like `0 1` and `0 2` are supported.
- Fully overlapping combos like `0 1` and `0 1 2` are supported.
- You are not limited to `&kp` bindings. You can use all ZMK behaviors there, like `&mo`, `&bt`, `&mt`, `&lt` etc.

:::note[Source-specific behaviors on split keyboards]
Invoking a [source-specific behavior](../features/split-keyboards.md#source-locality-behaviors) such as one of the [reset behaviors](behaviors/reset.md) using a combo will always trigger it on the central side of the keyboard, regardless of the side that the keys corresponding to `key-positions` are on.
:::

See [combo configuration](../config/combos.md) for advanced configuration options.

```

```
