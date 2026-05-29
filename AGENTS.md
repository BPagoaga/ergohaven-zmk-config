# Agent Guidelines

## Repository Purpose

This is a ZMK firmware user config for the [Ergohaven](https://github.com/ergohaven) keyboard family. It defines keymaps, combos, behaviors, and firmware settings for several keyboards. The keymap files are primarily edited via the [keymap-editor](https://nickcoutsos.github.io/keymap-editor/) web UI, which reads and writes `.keymap` and `.json` files.

## Key Hardware Context

- **Primary keyboard**: Omega 36 (`op36`) — 36-key columnar split
- **Host OS**: macOS with **French PC (AZERTY)** input layout
- **Firmware**: ZMK (Zephyr-based)

## French PC (AZERTY) Keycode Mapping Notes

ZMK keycodes are defined for a US QWERTY layout. When the host OS uses French PC (AZERTY), many keycodes produce different characters. The following mappings apply on macOS with the French PC layout:

| ZMK keycode | Expected | Actual on FR-PC macOS | Correct ZMK keycode |
|---|---|---|---|
| `CARET` (`LS(N6)`) | `^` | `¨` | `LBKT` |
| `DOLLAR` (`LS(N4)`) | `$` | `£` | `RBKT` |
| `KP_ASTERISK` | `*` | `8` | `NUHS` |
| `LBKT` | `[` | `^` (dead) | — |
| `LS(LBKT)` | `{` | `¨` (dead) | — |

When adding or modifying keys in `config/op36.keymap`, always verify that the raw ZMK HID keycode produces the intended character under the French PC layout, not the US layout.

## File Structure

```
config/
  op36.keymap          # Primary keymap — edit this for OP36 layout changes
  op36.json            # Physical key positions for keymap-editor
  op36.conf            # Firmware config (BLE, sleep, combos)
  west.yml             # Zephyr west manifest (pulls ergohaven-zmk@main)
  keys_ru.h            # Russian locale key definitions (not used for op36 default)
build.yaml             # CI build matrix
.github/workflows/
  build.yml            # GitHub Actions — delegates to ergohaven upstream workflow
```

## Keymap Layers (op36.keymap)

| Index | Name | Purpose |
|---|---|---|
| 0 | Base | QWERTY with Home Row Mods (HRM) |
| 1 | Sym | Symbols — tuned for FR-PC macOS |
| 2 | Num | Numpad |
| 3 | Nav | Arrows, media, brightness |
| 4 | BT | Bluetooth, F-keys, firmware controls |

## Editing Guidelines

- **Keymap editor**: open [keymap-editor](https://nickcoutsos.github.io/keymap-editor/) and point it to this repo. Edits are committed back to the repo.
- **Manual edits**: modify `config/op36.keymap` directly using ZMK DTS syntax.
- **Always account for FR-PC layout** when selecting keycodes for the Sym layer; use raw keycodes (`N6`, `LBKT`, `RA(...)`, etc.) rather than named aliases that assume US QWERTY.
- **AltGr sequences**: `RA(key)` sends Right Alt + key, which on FR-PC layout produces special characters (e.g., `RA(N4)` = `{`, `RA(EQUAL)` = `}`, `RA(N6)` = `|`).
- **Do not modify** `west.yml` or `.github/workflows/build.yml` unless changing the upstream ZMK fork.
- **Combos** are defined in the `combos` node of `op36.keymap` and use key position indices, not key labels.
