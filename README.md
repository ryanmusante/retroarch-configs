# retroarch-configs

[![version](https://img.shields.io/badge/version-4.1-blue.svg)](CHANGELOG.md)
[![cores](https://img.shields.io/badge/cores-7-green.svg)](#supported-cores)
[![paired](https://img.shields.io/badge/paired-retroarch--appletv4k%20v4.1-orange.svg)](https://github.com/ryanmusante/retroarch-appletv4k)
![license](https://img.shields.io/badge/license-MIT-green.svg)

> Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for
> Apple TV 4K. Companion to retroarch-appletv4k, which provides the
> global `retroarch.cfg`.

**Target:** RetroArch v1.22.x on tvOS 26 / Apple TV 4K 3rd Gen. See [Supported Cores](#supported-cores).

---

## Contents

- [Quick Start](#quick-start)
- [Supported Cores](#supported-cores)
- [Layout](#layout)
- [Configuration](#configuration)
- [Overclocking](#overclocking)
- [Per-Game Overrides](#per-game-overrides)
- [Related](#related)
- [Versioning](#versioning)
- [License](#license)

## Quick Start

1. Upload all 14 files from `config/` via the web interface or WebDAV.
2. On-device, create `config/<core_name>/` directories and move each `.cfg` / `.opt` pair into its core directory (see [Layout](#layout)).
3. Load content. Verify via Quick Menu → Overrides.

> [!IMPORTANT]
> If overrides are not at the per-core path, the global
> `run_ahead_enabled = "false"` wins and Tier 1 run-ahead will not
> activate.

See [CHANGELOG](CHANGELOG.md) for release history.

## Supported Cores

| Core | Systems | Tier | `.cfg` | `.opt` | Notes |
|------|---------|------|--------|--------|-------|
| Beetle PCE Fast | PC Engine / TG-16 | 1 | 2 | 2 | 2× CD streaming; no sprite limit; Run Ahead single-instance. Full CD-image RAM precache is per-game opt-in (imprudent as a global default on the 4 GB target) |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 | 3 | 0 | Run Ahead 2 single-instance; `rewind_enable = "false"` ([#16374](https://github.com/libretro/RetroArch/issues/16374)) |
| Genesis Plus GX | Genesis / MD / Sega CD / SMS | 1 | 2 | 3 | No sprite limit; per-game BRAM (system + cart); Run Ahead |
| Mesen | NES | 1 | 2 | 2 | No sprite limit; DMC popping correction off (revert per-game); Run Ahead |
| mGBA | GB / GBC / GBA | 1 | 2 | 1 | `mgba_color_correction = "Auto"`; Run Ahead. LCD look via `handheld/lcd-grid-v2.slangp` per-core |
| Snes9x | SNES | 1 | 2 | 1 | Reduce sprite flicker; Run Ahead |
| Mupen64Plus-Next | Nintendo 64 | 2 | 8 | 9 | tvOS Metal-only stack: angrylion sw-RDP + cxd4 RSP (no GL/Vulkan/JIT). `angrylion-multithread = "2"` (P-core pin, A15 2P+3E); `FrameDuping = "True"`; `pak1-4 = "rumble"`. Frontend pins per [Configuration](#configuration) |

## Layout

RetroArch loads per-core overrides from `config/<core_name>/` under
the RetroArch config root — **not** from this repo's flat `config/`
folder. On tvOS the config root is `Documents/RetroArch/`, exposed as
`config/` from the web / WebDAV root.

For each shipped core, create a directory named exactly after the
core (spaces included) and place both `.cfg` and `.opt` inside.

```
config/Mesen/
├── Mesen.cfg
└── Mesen.opt
```

Repeat for `Beetle PCE Fast`, `FinalBurn Neo`, `Genesis Plus GX`,
`Mupen64Plus-Next`, `Snes9x`, `mGBA`.

<details>
<summary><b>Zip contents (flat)</b></summary>

```
config/
├── Beetle PCE Fast.cfg / .opt
├── FinalBurn Neo.cfg / .opt
├── Genesis Plus GX.cfg / .opt
├── Mesen.cfg / .opt
├── Mupen64Plus-Next.cfg / .opt
├── Snes9x.cfg / .opt
└── mGBA.cfg / .opt
```

The zip ships files flat under `config/`. On-device, files must be
moved into per-core directories as shown above.

</details>

## Configuration

| File | Contents | Set via |
|------|----------|---------|
| `<core>.cfg` | Frontend (video, audio, latency, input) | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | Core emulation (renderer, CPU mode, accuracy) | Quick Menu → Options |

> [!WARNING]
> Mixing the two in one file causes silent failures — RetroArch
> ignores core option keys in `.cfg` and vice versa.

<details>
<summary><b>Frontend override keys</b></summary>

| Key | Values | Purpose |
|-----|--------|---------|
| `run_ahead_enabled` | `true`, `false` | Tier 1 per-core `true`; Tier 2 (Mupen) explicit `false` (HW-GL serialize breakage) |
| `run_ahead_secondary_instance` | `true`, `false` | Tier 2 (Mupen) explicit `false`; all other cores inherit global `false` for single-instance runahead |
| `video_threaded` | `false` | Tier 2 anchor ([#14978](https://github.com/libretro/RetroArch/issues/14978)) |
| `audio_latency` | `64` | Mupen +16 ms over global 48 (paired with `audio_sync` + FrameDuping) |
| `audio_sync` | `true` | Tier 2 mirrors global; DRC pitch shift instead of frame drops |
| `autosave_interval` | `0` | Tier 2 pin; prevents purgeable-cache stall from SRAM write |
| `video_scale_integer_scaling` | `1` | All Tier 1; integer overscale at 4K |
| `video_frame_delay_auto` | `false` | Mupen ([#14201](https://github.com/libretro/RetroArch/issues/14201)) |
| `rewind_enable` | `false` | FBN ([#16374](https://github.com/libretro/RetroArch/issues/16374)) + Mupen ([#18300](https://github.com/libretro/RetroArch/issues/18300)) |

Keys not set per-core (inherited from global):
`preemptive_frames_enable`, `audio_resampler_quality`,
`run_ahead_hide_warnings`, `run_ahead_frames`. `video_shader` is unset
everywhere; users assign per-core via Save Core Preset.

</details>

<details>
<summary><b>Shaders</b></summary>

Global `retroarch.cfg` enables the shader pipeline
(`video_shader_enable = "true"`) but sets no preset; none of the 7
`.cfg` files set one either. `crt/zfast-crt.slangp` is the
recommended starting point (single-pass, integer-scale safe). mGBA
users wanting LCD look should apply `handheld/lcd-grid-v2.slangp` via
Save Core Preset.

</details>

## Overclocking

CPU clock keys are not set globally — a value that fixes one title
breaks another. Apply per-game via Quick Menu → Core Options, then
Save Game Options.

| Core | Key | Values | Default |
|------|-----|--------|---------|
| Mesen | `mesen_overclock` | `None`, `Low`, `Medium`, `High`, `Very High` | `None` |
| Snes9x | `snes9x_overclock_superfx` | `50%`–`500%` | `100%` |
| Snes9x | `snes9x_overclock_cycles` | `disabled`, `light`, `compatible`, `max` | `disabled` |

## Per-Game Overrides

Create per-game files in the same `config/<core_name>/` directory.

Example — Run Ahead for Super Mario 64
(`config/Mupen64Plus-Next/Super Mario 64 (USA).cfg`):

```
run_ahead_enabled = "true"
run_ahead_frames = "1"
```

## Related

- [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k) — Global `retroarch.cfg` and Apple TV 4K setup guide

## Versioning

`vMAJOR.MINOR` (no patch), in lockstep with `retroarch-appletv4k` —
both repos share one tag per release. `MAJOR` on incompatible
structural changes; `MINOR` on every release. `CHANGELOG.md` is
kernel.org style and retains the last 5 MINOR entries.

## License

MIT © 2026 Ryan Musante
