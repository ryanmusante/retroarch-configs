# retroarch-configs

![version](https://img.shields.io/badge/version-1.25-blue)
![cores](https://img.shields.io/badge/cores-8-green)
![license](https://img.shields.io/badge/license-MIT-green)

Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for Apple TV 4K. Companion to [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k), which provides the global `retroarch.cfg`.

Overrides keep only non-global frontend keys. `.opt` files keep only non-default core settings.

[changelog](CHANGELOG.md)

## Table of Contents

- [Supported Cores](#supported-cores)
- [File Structure](#file-structure)
- [File Separation](#file-separation)
- [Frontend Override Keys](#frontend-override-keys)
- [CRT Shaders](#crt-shaders)
- [Installation](#installation)
- [Overclocking](#overclocking)
- [Per-Game Overrides](#per-game-overrides)
- [Related](#related)
- [Versioning](#versioning)
- [License](#license)

## Supported Cores

| Core | Systems | Tier | `.cfg` Keys | `.opt` Keys | CRT Shader | Notes |
|------|---------|------|-------------|-------------|------------|-------|
| Beetle PCE Fast | PC Engine / TurboGrafx-16 | 1 (Flawless) | 3 | 2 | crt-easymode (global) | Integer overscale for 240p at 4K; Run Ahead enabled per-core |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 (Flawless) | 2 | — | crt-easymode (global) | Defaults sufficient; rewind conflicts with Run Ahead (#16374); Run Ahead enabled per-core |
| Genesis Plus GX | Genesis / Mega Drive / Sega CD / Master System | 1 (Flawless) | 3 | 6 | crt-easymode (global) | MAME YM2612 (thermal-safe baseline; switch to Nuked per-game for audiophile titles); integer overscale for 224p; per-game BRAM isolation for Sega CD; Run Ahead enabled per-core |
| Mesen | NES | 1 (Flawless) | 3 | 2 | crt-easymode (global) | Integer overscale for 224p at 4K; Run Ahead enabled per-core |
| mGBA | GB / GBC / GBA | 1 (Flawless) | 3 | 3 | crt-easymode (global) | Integer overscale for GBA 240×160 at 4K; Run Ahead enabled per-core |
| Snes9x | SNES | 1 (Flawless) | 3 | 1 | crt-easymode (global) | Integer overscale for 224p at 4K; Run Ahead enabled per-core |
| Mupen64Plus-Next | Nintendo 64 | 2 (Good) | 7 | 12 | zfast_crt (override) | No JIT on tvOS; cached interpreter; Angrylion + CXD4; Player 1 Rumble Pak; CopyDepthToRDRAM Off (per-game Software for OoT/MM/Conker/Body Harvest); Run Ahead disabled by default |
| PCSX-ReARMed | PlayStation 1 | 2 (Good) | 7 | 4 | zfast_crt (override) | No JIT on tvOS; psxclock 100 native (per-game underclock for demanding 3D titles); async GPU threading; Run Ahead disabled by default | 2 (Good) | 7 | 4 | zfast_crt | No JIT on tvOS; Run Ahead disabled by default |

## File Structure

The ZIP itself ships the files flat under `config/`. That matches the archive contents.

```
config/
├── Beetle PCE Fast.cfg
├── Beetle PCE Fast.opt
├── FinalBurn Neo.cfg
├── Genesis Plus GX.cfg
├── Genesis Plus GX.opt
├── Mesen.cfg
├── Mesen.opt
├── mGBA.cfg
├── mGBA.opt
├── Mupen64Plus-Next.cfg
├── Mupen64Plus-Next.opt
├── PCSX-ReARMed.cfg
├── PCSX-ReARMed.opt
├── Snes9x.cfg
└── Snes9x.opt
```

For on-device placement, see `retroarch-appletv4k/README.md` §4 Filesystem layout.

README.md is the authoritative reference for archive layout and on-device per-core placement. Global frontend defaults come from the companion retroarch-appletv4k repository. The `.cfg` and `.opt` files intentionally omit layout/path notes.

The shipped ZIP contains 8 `.cfg`, 7 `.opt`, and `config/.gitkeep`. The filename list above reflects the functional override files; after upload, place each `.cfg` / `.opt` pair into its matching `config/<core_name>/` directory on the Apple TV for automatic loading.

## File Separation

RetroArch loads overrides and core options from separate files with distinct purposes.

| File | Path | Contents | Set via |
|------|------|----------|---------|
| `<core>.cfg` | Archive: `config/`  ·  Device: `config/<core_name>/` | RetroArch frontend settings (video, audio, latency, input) | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | Archive: `config/`  ·  Device: `config/<core_name>/` | Core-specific emulation options (renderer, CPU mode, accuracy) | Quick Menu → Options |

Mixing the two in a single file causes silent failures — RetroArch ignores core option keys in `.cfg` files and vice versa.

## Frontend Override Keys

Keys used across `.cfg` files and their purpose.

| Key | Values Used | Purpose |
|-----|-------------|---------|
| `run_ahead_enabled` | `true`, `false` | Tier 1: enabled per-core where validated; Tier 2: disabled by default |
| `run_ahead_frames` | `0`, `2` | Tier 1: 2-frame Run Ahead for reduced input lag once enabled per-core; Tier 2: disabled (0) without JIT |
| `preemptive_frames_enable` | `false` | Disabled for interpreter-bound cores (N64, PS1) |
| `video_frame_delay_auto` | `false` | Disabled for cores incompatible with auto frame delay |
| `video_threaded` | `true` | Threaded video for interpreter-bound cores (global = false). `PCSX-ReARMed.cfg` keeps `video_threaded = "true"` as a core-specific fallback for interpreter performance; if you observe pacing issues on your hardware, test `video_threaded = "false"` for that core only. The package does not apply that change by default. |
| `audio_latency` | `48`, `64` | Per-core: 64 ms for N64 (heavier interpreter), 48 ms for PS1 (minimum stable) |
| `audio_resampler_quality` | `2` | Lower resampler for Tier 2 cores (global = 3) |
| `video_scale_integer_scaling` | `1` | Overscale for 224p/240p content (NES, SNES, Genesis, PCE, GBA) at 4K |

## CRT Shaders

Global default `crt-easymode.slangp` is set in `retroarch-appletv4k/retroarch.cfg`. Tier 2 cores (Mupen64Plus-Next, PCSX-ReARMed) override to `zfast_crt.slangp` in their `.cfg` files to fit the interpreter + software-RDP GPU budget. See `retroarch-appletv4k/README.md` §8 for shader tuning guidance.

## Installation

> **Note:** The ZIP ships all `.cfg` and `.opt` files flat in `config/`. That is intentional. README.md is the authoritative source for archive layout and target on-device placement; global frontend defaults come from the companion `retroarch.cfg` repository.

On Apple TV, create `config/<core_name>/` directories, then move each matching `.cfg` / `.opt` pair into its core-named directory before launch. Tier 1 `.cfg` files now enable Run Ahead explicitly per core; the companion global `retroarch.cfg` remains conservative and does not enable it globally.

The override hierarchy applies automatically — no manual loading required. After uploading, launch a game with any configured core and verify via Quick Menu → Information that the override is active.

## Overclocking

CPU overclocking (`mesen_overclock_rate`, `snes9x_overclock`, `pcsx_rearmed_psxclock`) is intentionally **not** set globally — values that help one game break another. Apply per-game via Quick Menu → Core Options, then Save Game Options.

## Per-Game Overrides

For titles that need individual tuning, create per-game files in the same `config/<core_name>/` directory.

**Example — enabling Run Ahead for Super Mario 64:**

`config/Mupen64Plus-Next/Super Mario 64 (USA).cfg`:
```
run_ahead_enabled = "true"
run_ahead_frames = "1"
```

## Related

- [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k) — Global `retroarch.cfg` and Apple TV 4K setup guide

## Versioning

This repository uses `vMAJOR.MINOR` (no patch component). `MAJOR` increments on incompatible structural changes (filesystem layout, breaking config schema, removed features). `MINOR` increments on every release — additive changes, key additions/removals, documentation syncs, and conservative defaults adjustments. Each release ships with a matching `CHANGELOG.md` entry in kernel.org `date<TAB>name` style.

## License

[MIT](LICENSE)
