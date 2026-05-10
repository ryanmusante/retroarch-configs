# retroarch-configs

![version](https://img.shields.io/badge/version-3.28-blue)
![cores](https://img.shields.io/badge/cores-7-green)
![license](https://img.shields.io/badge/license-MIT-green)

Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for Apple TV 4K. Companion to [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k), which provides the global `retroarch.cfg`.

`.cfg` files keep only non-global frontend keys plus explicit drift-guard pins. `.opt` files keep only non-default core settings. No `video_shader` is set anywhere — users assign presets via Quick Menu → Shaders → Save Core Preset.

See [CHANGELOG](CHANGELOG.md) for release history.

## Table of Contents

1. [Supported Cores](#1-supported-cores)
2. [File Structure](#2-file-structure)
3. [File Separation](#3-file-separation)
4. [Frontend Override Keys](#4-frontend-override-keys)
5. [Shaders](#5-shaders)
6. [Installation](#6-installation)
7. [Manual Install: Per-Core Override Path](#7-manual-install-per-core-override-path)
8. [Overclocking](#8-overclocking)
9. [Per-Game Overrides](#9-per-game-overrides)
10. [Related](#10-related)
11. [Versioning](#11-versioning)
12. [License](#12-license)

## 1. Supported Cores

| Core | Systems | Tier | `.cfg` | `.opt` | Notes |
|------|---------|------|--------|--------|-------|
| Beetle PCE Fast | PC Engine / TG-16 | 1 | 2 | 3 | CD precache + 2× streaming (per upstream advisory + community-standard reference); no sprite limit; Run Ahead single-instance |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 | 3 | 0 | Run Ahead 2 single-instance (per upstream FBN libretro README); `rewind_enable = "false"` ([#16374](https://github.com/libretro/RetroArch/issues/16374)) |
| Genesis Plus GX | Genesis / MD / Sega CD / SMS | 1 | 2 | 3 | No sprite limit; per-game BRAM (system + cart); Run Ahead |
| Mesen | NES | 1 | 2 | 2 | No sprite limit; DMC popping correction off (revert per-game); Run Ahead |
| mGBA | GB / GBC / GBA | 1 | 2 | 1 | `mgba_color_correction = "Auto"` (per-system correction); Run Ahead. LCD look via `handheld/lcd-grid-v2.slangp` per-core |
| Snes9x | SNES | 1 | 2 | 1 | Reduce sprite flicker; Run Ahead |
| Mupen64Plus-Next | Nintendo 64 | 2 | 8 | 9 | tvOS Metal-only stack: angrylion sw-RDP + cxd4 RSP (no GL/Vulkan/JIT). `mupen64plus-angrylion-multithread = "2"` (P-core pin on A15 2P+3E binned); `mupen64plus-FrameDuping = "True"` (Switch-parity smoothing); `mupen64plus-pak1-4 = "rumble"` (4P parity). Frontend pins per [§4](#4-frontend-override-keys) |

## 2. File Structure

```
config/
├── Beetle PCE Fast.cfg
├── Beetle PCE Fast.opt
├── FinalBurn Neo.cfg
├── FinalBurn Neo.opt
├── Genesis Plus GX.cfg
├── Genesis Plus GX.opt
├── Mesen.cfg
├── Mesen.opt
├── Mupen64Plus-Next.cfg
├── Mupen64Plus-Next.opt
├── Snes9x.cfg
├── Snes9x.opt
├── mGBA.cfg
└── mGBA.opt
```

The ZIP ships files flat under `config/`. On-device placement into per-core directories (`config/<core_name>/`) is covered in [§7](#7-manual-install-per-core-override-path) — single source of truth for on-device layout.

## 3. File Separation

| File | Contents | Set via |
|------|----------|---------|
| `<core>.cfg` | Frontend settings (video, audio, latency, input) | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | Core emulation options (renderer, CPU mode, accuracy) | Quick Menu → Options |

Mixing the two in one file causes silent failures — RetroArch ignores core option keys in `.cfg` and vice versa.

## 4. Frontend Override Keys

| Key | Values | Purpose |
|-----|--------|---------|
| `run_ahead_enabled` | `true`, `false` | Tier 1 per-core `true`; Tier 2 (Mupen) explicit `false` (HW-GL serialize breakage) |
| `run_ahead_secondary_instance` | `true`, `false` | Tier 2 (Mupen) explicit `false`; all other cores inherit global `false` for single-instance runahead per upstream FBN libretro README |
| `video_threaded` | `false` | Tier 2 anchor ([#14978](https://github.com/libretro/RetroArch/issues/14978)) |
| `audio_latency` | `64` | Mupen +16 ms over global 48 (paired with `audio_sync` + FrameDuping) |
| `audio_sync` | `true` | Tier 2 mirrors global; DRC pitch shift instead of frame drops |
| `autosave_interval` | `0` | Tier 2 pin; prevents purgeable-cache stall from SRAM write |
| `video_scale_integer_scaling` | `1` | All Tier 1; integer overscale at 4K |
| `video_frame_delay_auto` | `false` | Mupen ([#14201](https://github.com/libretro/RetroArch/issues/14201)) |
| `rewind_enable` | `false` | FBN ([#16374](https://github.com/libretro/RetroArch/issues/16374)) + Mupen ([#18300](https://github.com/libretro/RetroArch/issues/18300)) |

Keys not set per-core (inherited from global): `preemptive_frames_enable`, `audio_resampler_quality`, `run_ahead_hide_warnings`, `run_ahead_frames`. `video_shader` is unset everywhere; users assign per-core via Save Core Preset.

## 5. Shaders

Global `retroarch.cfg` enables the shader pipeline (`video_shader_enable = "true"`) but sets no preset; none of the 7 `.cfg` files set one either. `crt/zfast-crt.slangp` is the recommended starting point (single-pass, integer-scale safe). mGBA users wanting LCD look should apply `handheld/lcd-grid-v2.slangp` via Save Core Preset.

## 6. Installation

Create `config/<core_name>/` directories under the RetroArch config root, then move each `.cfg` / `.opt` pair into its core-named directory before launch (see [§7](#7-manual-install-per-core-override-path)). The override hierarchy applies automatically — verify via Quick Menu → Information.

## 7. Manual Install: Per-Core Override Path

RetroArch loads per-core overrides from `config/<core_name>/` under the RetroArch config root — **not** from this repo's flat `config/` folder. On tvOS the config root maps to `Documents/RetroArch/` in the app sandbox, exposed as `config/` from the WebDAV / web root.

For each shipped core, create a directory named exactly after the core (spaces included) and place both `.cfg` and `.opt` inside. Example for Mesen:

```
config/Mesen/
├── Mesen.cfg
└── Mesen.opt
```

Repeat for the other 6 cores (`Beetle PCE Fast`, `FinalBurn Neo`, `Genesis Plus GX`, `Mupen64Plus-Next`, `Snes9x`, `mGBA`).

**Verify:** Quick Menu → Overrides → core-override entry should be populated after loading content. If empty, the path is wrong. If overrides aren't installed, the global `run_ahead_enabled = "false"` wins and Tier 1 run-ahead won't activate.

## 8. Overclocking

CPU clock keys are not set globally — a value that fixes one title breaks another. Apply per-game via Quick Menu → Core Options, then Save Game Options.

| Core | Key | Values | Default |
|------|-----|--------|---------|
| Mesen | `mesen_overclock` | `None`, `Low`, `Medium`, `High`, `Very High` | `None` |
| Snes9x | `snes9x_overclock_superfx` | `50%`–`500%` | `100%` |
| Snes9x | `snes9x_overclock_cycles` | `disabled`, `light`, `compatible`, `max` | `disabled` |

## 9. Per-Game Overrides

Create per-game files in the same `config/<core_name>/` directory.

**Example — Run Ahead for Super Mario 64** (`config/Mupen64Plus-Next/Super Mario 64 (USA).cfg`):

```
run_ahead_enabled = "true"
run_ahead_frames = "1"
```

## 10. Related

- [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k) — Global `retroarch.cfg` and Apple TV 4K setup guide

## 11. Versioning

`vMAJOR.MINOR` (no patch) in lockstep with `retroarch-appletv4k`; both repos share one tag and bump together every release. `MAJOR` increments on incompatible structural changes; `MINOR` increments on every release. `CHANGELOG.md` is kernel.org style (`# Version - Date` + bullets) and retains the last 5 MINOR entries.

## 12. License

[MIT](LICENSE)
