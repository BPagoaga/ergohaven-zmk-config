# ergohaven-zmk-config

ZMK firmware user config for the [Ergohaven](https://github.com/ergohaven) keyboard family.

## Hardware

| Keyboard | Keys | Notes |
|---|---|---|
| [Omega 36](https://eh.industries/op36) (`op36`) | 36 | Columnar split — primary keyboard |
| Velvet v3 (`velvet_v3`) | 46 | Split ergo |
| Velvet v3 UI (`velvet_v3_ui`) | 46 | Velvet v3 with integrated trackball |
| K03 (`k03`) | ~60 | Split with rotary encoder |
| Imperial44 (`imperial44`) | 44 | Split with dual encoders |
| Trackball v3.0 / v3.1 / Royal | — | Peripheral trackball modules |

## Editing the Keymap

The primary editing workflow is through the [keymap-editor](https://nickcoutsos.github.io/keymap-editor/) web UI:

1. Go to [nickcoutsos.github.io/keymap-editor](https://nickcoutsos.github.io/keymap-editor/)
2. Connect to this GitHub repository
3. Edit layers visually — changes are committed back to the repo
4. GitHub Actions builds the firmware automatically on push

For manual edits, modify `config/op36.keymap` (or the relevant keyboard's `.keymap` file) directly using ZMK DTS syntax.

## Omega 36 Keymap Layers

The `op36` keymap has 5 layers:

| # | Name | Purpose |
|---|---|---|
| 0 | Base | QWERTY with Home Row Mods (HRM) |
| 1 | Sym | Symbols, tuned for FR-PC macOS AZERTY |
| 2 | Num | Numpad |
| 3 | Nav | Arrows, media, brightness |
| 4 | BT | Bluetooth profiles, F-keys, firmware |

### Home Row Mods

The Base layer uses `hml`/`hmr` balanced hold-tap behaviors:
- Left hand: `A`=GUI, `S`=Alt, `D`=Ctrl, `F`=Shift
- Right hand: `J`=Shift, `K`=Ctrl, `L`=Alt, `;`=GUI

### Combos

| Keys (positions) | Output |
|---|---|
| `Q` + `A` (0+10) | Escape |
| `C` + `V` (22+23) | `ç` |
| `E` + `D` (2+12) | `é` |
| `R` + `F` (3+13) | `è` |
| `F` + `J` (13+16) | Caps Word |

## French PC (AZERTY) Layout Notes

This config targets macOS with the **French PC (AZERTY)** input layout. ZMK keycodes are defined for US QWERTY, so many keycodes produce different characters than their names suggest. Key corrections used in the Sym layer:

| ZMK keycode | FR-PC output | Notes |
|---|---|---|
| `LBKT` | `^` | `CARET` (`LS(N6)`) gives `¨` instead |
| `RBKT` | `$` | `DOLLAR` (`LS(N4)`) gives `£` instead |
| `NUHS` | `*` | `KP_ASTERISK` gives `8` instead |
| `RA(N4)` | `{` | AltGr+4 |
| `RA(EQUAL)` | `}` | AltGr+= |
| `RA(N6)` | `\|` | AltGr+6 |
| `RA(N5)` | `[` | AltGr+5 |
| `RA(MINUS)` | `]` | AltGr+- |

## Building Firmware

Firmware is built automatically via GitHub Actions on every push. The workflow delegates to the upstream [ergohaven-zmk](https://github.com/ergohaven/ergohaven-zmk) reusable workflow.

To trigger a manual build: **Actions** tab → **Build** → **Run workflow**.

Firmware `.uf2` files are available as build artifacts.

## Repository Structure

```
config/
  op36.keymap        # OP36 keymap (primary)
  op36.json          # OP36 physical layout for keymap-editor
  op36.conf          # OP36 firmware config
  west.yml           # Zephyr west manifest
  keys_ru.h          # Russian locale key definitions
  velvet_v3.*        # Velvet v3 keymap/config/layout
  velvet_v3_ui.*     # Velvet v3 UI (trackball) variant
  k03.*              # K03 keymap/config/layout
  imperial44.*       # Imperial44 keymap/config/layout
  trackball_*        # Trackball peripheral keymaps
build.yaml           # CI build matrix
.github/workflows/
  build.yml          # GitHub Actions CI
```
