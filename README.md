# retroarch-configs

![version](https://img.shields.io/badge/version-1.49-blue)
![cores](https://img.shields.io/badge/cores-8-green)
![license](https://img.shields.io/badge/license-MIT-green)

Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for Apple TV 4K. Companion to [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k), which provides the global `retroarch.cfg`.

Overrides keep only non-global frontend keys, except for explicit pins that guard against global drift: Tier 2 `video_threaded = "false"` ([#14978](https://github.com/libretro/RetroArch/issues/14978) Apple-platform anchor), Tier 2 `run_ahead_enabled = "false"` (PCSX-ReARMed interpreter safety; Mupen64Plus-Next HW-GL context serialize/unserialize breakage), `run_ahead_secondary_instance` pinned across all 8 cores (7 static cores = `"false"`, FBN = `"true"` per core maintainer), and `video_scale_integer = "true"` mirroring the global pin (drift-guard, not enable gate — global `retroarch.cfg` already sets `true`) paired with the per-core `video_scale_integer_scaling` mode selector. Shader assignment is global (set in companion `retroarch.cfg`); per-core `.cfg` files do not set `video_shader`. `.opt` files keep only non-default core settings.

[changelog](CHANGELOG.md)

## Table of Contents

1. [Supported Cores](#1-supported-cores)
2. [File Structure](#2-file-structure)
3. [File Separation](#3-file-separation)
4. [Frontend Override Keys](#4-frontend-override-keys)
5. [Shaders](#5-shaders)
6. [Installation](#6-installation)
7. [Manual Install: Per-Core Override Path](#7-manual-install-per-core-override-path)
   - [Apple TV / tvOS](#apple-tv--tvos)
8. [Overclocking](#8-overclocking)
9. [Per-Game Overrides](#9-per-game-overrides)
10. [Related](#10-related)
11. [Versioning](#11-versioning)
12. [License](#12-license)

## 1. Supported Cores

| Core | Systems | Tier | `.cfg` Keys | `.opt` Keys | Notes |
|------|---------|------|-------------|-------------|-------|
| Beetle PCE Fast | PC Engine / TurboGrafx-16 | 1 (Flawless) | 6 | 3 | Integer overscale 256×240 (+ 512-wide hi-res) @ 4K; Run Ahead 1 frame (CDROM seek determinism) |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 (Flawless) | 7 | 0 | Integer overscale 224p–304p @ 4K; `run_ahead_secondary_instance = "true"` per maintainer ([#16374](https://github.com/libretro/RetroArch/issues/16374) savestate audio buzz); `rewind_enable = "false"`; `.opt` placeholder (dipswitch/cheat per-game only) |
| Genesis Plus GX | Genesis / Mega Drive / Sega CD / Master System | 1 (Flawless) | 6 | 7 | MAME YM2612 (thermal-safe; Nuked per-game); integer overscale 224p @ 4K; per-game BRAM isolation for Sega CD; Run Ahead per-core |
| Mesen | NES | 1 (Flawless) | 6 | 2 | Integer overscale 224p @ 4K; Run Ahead per-core |
| mGBA | GB / GBC / GBA | 1 (Flawless) | 6 | 3 | Integer overscale GBA 240×160 @ 4K; `interframe_blending = "mix_smart"` (blends flicker-effect frames only; preserves motion clarity in non-flickering titles); Run Ahead per-core. Handheld LCDs — apply `handheld/lcd-grid-v2.slangp` via Quick Menu → Shaders → Save Core Preset for LCD aesthetic (see companion §8) |
| Snes9x | SNES | 1 (Flawless) | 6 | 1 | Integer overscale 224p @ 4K; Run Ahead per-core |
| Mupen64Plus-Next | Nintendo 64 | 2 (Good) | 5 | 6 | No JIT; cached interpreter; Angrylion sw RDP + CXD4; native 320×240; `mupen64plus-angrylion-multithread = "2"` (ATV4K binned A15 = 2P+3E; default "all threads" oversubscribes E-cores, harming frame pacing vs 2 P-core workers); per-core pins: `video_threaded = "false"` ([#14978](https://github.com/libretro/RetroArch/issues/14978)), `video_frame_delay_auto = "false"` ([#14201](https://github.com/libretro/RetroArch/issues/14201)), `rewind_enable = "false"` ([#18300](https://github.com/libretro/RetroArch/issues/18300)), `run_ahead_enabled = "false"` (HW-GL state thrash on serialize/unserialize) |
| PCSX-ReARMed | PlayStation 1 | 2 (Good) | 7 | 6 | No JIT; `psxclock = "100"` (per-game underclock for 3D); async GPU; `video_threaded = "false"` ([#14978](https://github.com/libretro/RetroArch/issues/14978)); `audio_latency = "48"`; `run_ahead_enabled = "false"` (interpreter safety); `rewind_enable = "false"` (defensive pin); integer overscale (variable width 256–640 may shift borders); `pcsx_rearmed_cd_readahead = "333000"` (full-disk precache; ~750 MB RAM; eliminates CHD seek latency) |

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
├── PCSX-ReARMed.cfg
├── PCSX-ReARMed.opt
├── Snes9x.cfg
├── Snes9x.opt
├── mGBA.cfg
└── mGBA.opt
```

The ZIP ships the files flat under `config/`. On-device placement into per-core directories (`config/<core_name>/`) is covered in [Manual Install: Per-Core Override Path](#7-manual-install-per-core-override-path) below — that section is the single source of truth for on-device layout. The `.cfg` and `.opt` files intentionally omit layout/path notes; global frontend defaults come from the companion `retroarch-appletv4k` repository.

The shipped ZIP contains 8 `.cfg` and 8 `.opt` files, flat under `config/`.

## 3. File Separation

RetroArch loads overrides and core options from separate files with distinct purposes.

| File | Path | Contents | Set via |
|------|------|----------|---------|
| `<core>.cfg` | Archive: `config/`  ·  Device: `config/<core_name>/` | Frontend settings (video, audio, latency, input) | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | Archive: `config/`  ·  Device: `config/<core_name>/` | Core emulation options (renderer, CPU mode, accuracy) | Quick Menu → Options |

Mixing the two in a single file causes silent failures — RetroArch ignores core option keys in `.cfg` files and vice versa.

## 4. Frontend Override Keys

Keys actually set in one or more shipped `.cfg` files.

| Key | Values Used | Purpose |
|-----|-------------|---------|
| `run_ahead_enabled` | `true`, `false` | Tier 1 per-core `true` (global is `false`); PCSX-ReARMed + Mupen64Plus-Next explicit `false` (interpreter safety; Mupen HW-GL context serialize/unserialize breakage) |
| `run_ahead_frames` | `1`, `2` | Tier 1 = 2; Beetle PCE Fast = 1 (CDROM seek determinism) |
| `run_ahead_secondary_instance` | `true`, `false` | All 7 static cores pinned `false` (static-build inert; prevents behavior change on dynamic rebuilds — runtime default is `true`); FBN pinned `true` per core maintainer ([#16374](https://github.com/libretro/RetroArch/issues/16374) savestate audio buzz) |
| `video_threaded` | `false` | Tier 2 forensic anchor ([#14978](https://github.com/libretro/RetroArch/issues/14978)); upstream `gfx/video_driver.c` force-disables for all Apple platforms |
| `audio_latency` | `48` | PCSX-ReARMed pin; matches global v2.66 (down from 64 ms); Mupen inherits global |
| `video_scale_integer` | `true` | All Tier 1 + PCSX-ReARMed; mirrors global `retroarch.cfg` pin as a drift-guard (not the enable gate — global already sets `true`). SETTING_BOOL, upstream default `false`; orthogonal to the `video_scale_integer_scaling` mode selector. Mupen64Plus-Next has no per-core pin and inherits global `true` (plain integer scale; no overscale mode) |
| `video_scale_integer_scaling` | `1` | All Tier 1 + PCSX-ReARMed; integer overscale mode at 4K. PS1 variable width (256–640) may shift borders on mode switch |
| `video_frame_delay_auto` | `true`, `false` | Tier 1 = `true` (global as of v2.66); Mupen = `false` ([#14201](https://github.com/libretro/RetroArch/issues/14201)); PCSX inherits |
| `rewind_enable` | `false` | FBN + Mupen + PCSX-ReARMed pins: [#16374](https://github.com/libretro/RetroArch/issues/16374) Run Ahead conflict; [#18300](https://github.com/libretro/RetroArch/issues/18300) N64 freeze; PCSX defensive guard against global menu toggle (PS1 rewind memory cost causes stalls on no-JIT A15) |

Keys intentionally *not* set per-core (inherited from global `retroarch.cfg`): `video_shader`, `preemptive_frames_enable`, `audio_resampler_quality`, `run_ahead_hide_warnings`.

## 5. Shaders

Global `retroarch.cfg` sets both the shader pipeline gate (`video_shader_enable = "true"`) and a global preset (`video_shader = "../shaders/shaders_slang/crt/crt-aperture.slangp"`) as of companion v2.84. None of the 8 per-core `.cfg` files set `video_shader` — every core inherits the global `crt-aperture` preset. Per-game or per-core shader overrides use RetroArch's Save Core Preset / Save Game Preset UI (see `retroarch-appletv4k/README.md` §8), which writes dedicated `.slangp` files rather than `.cfg` entries. mGBA users who prefer the handheld-LCD aesthetic should apply `../shaders/shaders_slang/handheld/lcd-grid-v2.slangp` via Save Core Preset.

## 6. Installation

On Apple TV, create `config/<core_name>/` directories under the RetroArch config root, then move each matching `.cfg` / `.opt` pair into its core-named directory before launch. See [Manual Install: Per-Core Override Path](#7-manual-install-per-core-override-path) for the full path reference. Tier 1 `.cfg` files enable Run Ahead explicitly per core; the companion global `retroarch.cfg` remains conservative and does not enable it globally.

The override hierarchy applies automatically — no manual loading required. After uploading, launch a game with any configured core and verify via Quick Menu → Information that the override is active.

## 7. Manual Install: Per-Core Override Path

RetroArch loads per-core overrides only from a specific directory under the RetroArch config root — **not** from this repo's `config/` folder. You must manually copy files into place after cloning.

### Apple TV / tvOS

RetroArch config root on tvOS: `Documents/RetroArch/config/` inside the app's sandboxed container.

For each core, create a directory named exactly after the core (spaces included) and place both the `.cfg` and `.opt` files inside:

```
Documents/RetroArch/config/
├── Beetle PCE Fast/
│   ├── Beetle PCE Fast.cfg
│   └── Beetle PCE Fast.opt
├── FinalBurn Neo/
│   ├── FinalBurn Neo.cfg
│   └── FinalBurn Neo.opt
├── Genesis Plus GX/
│   ├── Genesis Plus GX.cfg
│   └── Genesis Plus GX.opt
├── Mesen/
│   ├── Mesen.cfg
│   └── Mesen.opt
├── Mupen64Plus-Next/
│   ├── Mupen64Plus-Next.cfg
│   └── Mupen64Plus-Next.opt
├── PCSX-ReARMed/
│   ├── PCSX-ReARMed.cfg
│   └── PCSX-ReARMed.opt
├── Snes9x/
│   ├── Snes9x.cfg
│   └── Snes9x.opt
└── mGBA/
    ├── mGBA.cfg
    └── mGBA.opt
```

**Verify overrides loaded:** RetroArch menu → Quick Menu → Overrides → the core-override entry should be populated after loading content. If empty, the path is wrong.

If overrides are not installed, the global `run_ahead_enabled = "false"` in `retroarch.cfg` wins and Tier 1 run-ahead will not activate.

## 8. Overclocking

CPU clock adjustment keys are not set to non-default values globally. `mesen_overclock_rate` and `snes9x_overclock` are absent from all shipped files — a value that fixes one title breaks another. Apply per-game via Quick Menu → Core Options, then Save Game Options.

`pcsx_rearmed_psxclock` is the exception: `PCSX-ReARMed.opt` ships `"100"` as a native-clock anchor (not overclocking). Underclock values (75, 50) for demanding 3D titles (Tony Hawk, Spyro 2/3, Tekken 3) require per-game application.

## 9. Per-Game Overrides

For titles that need individual tuning, create per-game files in the same `config/<core_name>/` directory.

**Example — enabling Run Ahead for Super Mario 64:**

`config/Mupen64Plus-Next/Super Mario 64 (USA).cfg`:
```
run_ahead_enabled = "true"
run_ahead_frames = "1"
```

## 10. Related

- [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k) — Global `retroarch.cfg` and Apple TV 4K setup guide

## 11. Versioning

This repository uses `vMAJOR.MINOR` (no patch component). `MAJOR` increments on incompatible structural changes (filesystem layout, breaking config schema, removed features). `MINOR` increments on every release — additive changes, key additions/removals, documentation syncs, and conservative defaults adjustments. Each release ships with a matching `CHANGELOG.md` entry in GNU ChangeLog style (`YYYY-MM-DD<SP><SP>Author Name`; date and author separated by two spaces). `CHANGELOG.md` retains the last 5 MINOR version entries; older entries are trimmed on each release.

## 12. License

[MIT](LICENSE)
