# retroarch-configs

![version](https://img.shields.io/badge/version-1.29.1-blue)
![cores](https://img.shields.io/badge/cores-8-green)
![license](https://img.shields.io/badge/license-MIT-green)

Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for Apple TV 4K. Companion to [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k), which provides the global `retroarch.cfg`.

Overrides keep only non-global frontend keys, except for Tier 2 `video_threaded = "false"` which redundantly pins the global value as an explicit [#14978](https://github.com/libretro/RetroArch/issues/14978) crash-defense anchor. `.opt` files keep only non-default core settings.

[changelog](CHANGELOG.md)

## Table of Contents

1. [Supported Cores](#1-supported-cores)
2. [File Structure](#2-file-structure)
3. [File Separation](#3-file-separation)
4. [Frontend Override Keys](#4-frontend-override-keys)
5. [CRT Shaders](#5-crt-shaders)
6. [Installation](#6-installation)
7. [Manual Install: Per-Core Override Path](#7-manual-install-per-core-override-path)
8. [Overclocking](#8-overclocking)
9. [Per-Game Overrides](#9-per-game-overrides)
10. [Related](#10-related)
11. [Versioning](#11-versioning)
12. [License](#12-license)

## 1. Supported Cores

| Core | Systems | Tier | `.cfg` Keys | `.opt` Keys | CRT Shader | Notes |
|------|---------|------|-------------|-------------|------------|-------|
| Beetle PCE Fast | PC Engine / TurboGrafx-16 | 1 (Flawless) | 3 | 2 | crt-easymode (global) | Integer overscale for 240p at 4K; Run Ahead enabled per-core (1 frame; CDROM seek determinism) |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 (Flawless) | 3 | — | crt-easymode (global) | Integer overscale for 224p-304p at 4K; rewind remains globally disabled — do not re-enable per-game alongside Run Ahead ([#16374](https://github.com/libretro/RetroArch/issues/16374)); Run Ahead enabled per-core |
| Genesis Plus GX | Genesis / Mega Drive / Sega CD / Master System | 1 (Flawless) | 3 | 6 | crt-easymode (global) | MAME YM2612 (thermal-safe baseline; switch to Nuked per-game for audiophile titles); integer overscale for 224p; per-game BRAM isolation for Sega CD; Run Ahead enabled per-core |
| Mesen | NES | 1 (Flawless) | 3 | 2 | crt-easymode (global) | Integer overscale for 224p at 4K; Run Ahead enabled per-core |
| mGBA | GB / GBC / GBA | 1 (Flawless) | 3 | 3 | crt-easymode (global) | Integer overscale for GBA 240×160 at 4K; interframe_blending=mix (thermal/fillrate relief); Run Ahead enabled per-core |
| Snes9x | SNES | 1 (Flawless) | 3 | 1 | crt-easymode (global) | Integer overscale for 224p at 4K; Run Ahead enabled per-core |
| Mupen64Plus-Next | Nintendo 64 | 2 (Good) | 2 | 7 | zfast_crt (override) | No JIT on tvOS; cached interpreter; Angrylion software RDP + CXD4 RSP; Player 1 Rumble Pak; native 320×240 framebuffer; video_threaded=false ([#14978](https://github.com/libretro/RetroArch/issues/14978) defense); Run Ahead disabled by default |
| PCSX-ReARMed | PlayStation 1 | 2 (Good) | 3 | 4 | zfast_crt (override) | No JIT on tvOS; psxclock 100 native (per-game underclock for demanding 3D titles); async GPU threading; video_threaded=false ([#14978](https://github.com/libretro/RetroArch/issues/14978) defense); audio_latency=48 (lighter than N64); Run Ahead disabled by default |

## 2. File Structure

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

The ZIP ships the files flat under `config/`. On-device placement into per-core directories (`config/<core_name>/`) is covered in [Manual Install: Per-Core Override Path](#7-manual-install-per-core-override-path) below — that section is the single source of truth for on-device layout. The `.cfg` and `.opt` files intentionally omit layout/path notes; global frontend defaults come from the companion `retroarch-appletv4k` repository.

The shipped ZIP contains 8 `.cfg`, 7 `.opt`, and `config/.gitkeep`.

## 3. File Separation

RetroArch loads overrides and core options from separate files with distinct purposes.

| File | Path | Contents | Set via |
|------|------|----------|---------|
| `<core>.cfg` | Archive: `config/`  ·  Device: `config/<core_name>/` | RetroArch frontend settings (video, audio, latency, input) | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | Archive: `config/`  ·  Device: `config/<core_name>/` | Core-specific emulation options (renderer, CPU mode, accuracy) | Quick Menu → Options |

Mixing the two in a single file causes silent failures — RetroArch ignores core option keys in `.cfg` files and vice versa.

## 4. Frontend Override Keys

Keys actually set in one or more shipped `.cfg` files.

| Key | Values Used | Purpose |
|-----|-------------|---------|
| `run_ahead_enabled` | `true` | Tier 1 cores enable Run Ahead per-core (global baseline is `false`); Tier 2 cores leave it unset and inherit the global `false` |
| `run_ahead_frames` | `1`, `2` | Tier 1 = 2 frames, except **Beetle PCE Fast = 1** (CDROM seek state is not fully deterministic under second-instance-disabled run-ahead, so 1 is the safe ceiling) |
| `video_threaded` | `false` | Tier 2 (`Mupen64Plus-Next.cfg`, `PCSX-ReARMed.cfg`) redundantly pin `false` to match the global [#14978](https://github.com/libretro/RetroArch/issues/14978) Metal-crash defense. The effective value is `false` everywhere — this key is a forensic anchor, not a divergence |
| `audio_latency` | `48` | Only `PCSX-ReARMed.cfg` overrides (48 ms, lighter than the N64 interpreter). `Mupen64Plus-Next.cfg` does **not** set this key and inherits the global `64` ms |
| `video_shader` | `shaders_slang/crt/zfast_crt.slangp` | Tier 2 override of the global `crt-easymode.slangp` to fit the interpreter + software-RDP GPU budget |
| `video_scale_integer_scaling` | `1` | Tier 1 integer overscale for 224p–304p content at 4K (NES, SNES, Genesis, PCE, GBA, FBN) |

Keys intentionally *not* set per-core (inherited from global `retroarch.cfg`): `preemptive_frames_enable`, `video_frame_delay_auto`, `audio_resampler_quality`, `run_ahead_secondary_instance`, `run_ahead_hide_warnings`.

## 5. CRT Shaders

Global default `crt-easymode.slangp` is set in `retroarch-appletv4k/retroarch.cfg`. Tier 2 cores (Mupen64Plus-Next, PCSX-ReARMed) override to `zfast_crt.slangp` in their `.cfg` files to fit the interpreter + software-RDP GPU budget. See `retroarch-appletv4k/README.md` §8 for shader tuning guidance.

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
│   └── FinalBurn Neo.cfg
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

CPU overclocking (`mesen_overclock_rate`, `snes9x_overclock`, `pcsx_rearmed_psxclock`) is intentionally **not** set globally — values that help one game break another. Apply per-game via Quick Menu → Core Options, then Save Game Options.

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

This repository uses `vMAJOR.MINOR` (no patch component). `MAJOR` increments on incompatible structural changes (filesystem layout, breaking config schema, removed features). `MINOR` increments on every release — additive changes, key additions/removals, documentation syncs, and conservative defaults adjustments. Each release ships with a matching `CHANGELOG.md` entry in kernel.org `date<TAB>name` style.

## 12. License

[MIT](LICENSE)
