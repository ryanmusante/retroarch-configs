# retroarch-configs

![version](https://img.shields.io/badge/version-3.18-blue)
![cores](https://img.shields.io/badge/cores-8-green)
![license](https://img.shields.io/badge/license-MIT-green)

Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for Apple TV 4K. Companion to [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k), which provides the global `retroarch.cfg`.

`.cfg` files keep only non-global frontend keys plus explicit drift-guard pins (see §4). `.opt` files keep only non-default core settings. No `video_shader` is set anywhere — users assign presets via Quick Menu → Shaders → Save Core Preset (see §5).

See [CHANGELOG](CHANGELOG.md) for release history.

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

| Core | Systems | Tier | `.cfg` | `.opt` | Notes |
|------|---------|------|--------|--------|-------|
| Beetle PCE Fast | PC Engine / TG-16 | 1 | 2 | 3 | Integer overscale 256×240 @ 4K; Run Ahead 2 (single instance; CD-safe per [libretro/beetle-pce-fast-libretro#127](https://github.com/libretro/beetle-pce-fast-libretro/issues/127)); `pce_fast_cdimagecache = "enabled"`; `pce_fast_cdspeed = "4"` (may desync Ys IV / Dracula X — revert per-game); `pce_fast_nospritelimit = "enabled"` |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 | 4 | 0 | Integer overscale 224p–304p; Run Ahead 2 frames with `run_ahead_secondary_instance = "true"` ([#16374](https://github.com/libretro/RetroArch/issues/16374) — per core maintainer); `rewind_enable = "false"` ([#16374](https://github.com/libretro/RetroArch/issues/16374)); `.opt` placeholder (per-game only) |
| Genesis Plus GX | Genesis / MD / Sega CD / SMS | 1 | 2 | 6 | `genesis_plus_gx_ym2612 = "mame (ym2612)"`; `genesis_plus_gx_audio_filter = "disabled"` (v3.1 off; v3.2 enum fix); `genesis_plus_gx_no_sprite_limit = "enabled"`; `genesis_plus_gx_render = "single field"`; `genesis_plus_gx_system_bram = "per game"` + `genesis_plus_gx_cart_bram = "per game"`; integer overscale 224p; Run Ahead |
| Mesen | NES | 1 | 2 | 2 | Integer overscale 224p; `mesen_nospritelimit = "enabled"`; `mesen_reduce_dmc_popping = "disabled"` (pops possible on Skate or Die 2, Ninja Gaiden III — restore per-game); Run Ahead |
| mGBA | GB / GBC / GBA | 1 | 2 | 3 | Integer overscale GBA 240×160; `mgba_interframe_blending = "OFF"` (v3.9 enum fix — prior releases used `"disabled"` which is the UI label, not a valid key; core fell through to default "OFF" silently. Same class of bug as the v3.2 `color_correction` fix. Flicker on Zelda MC, F-Zero GP Legend, MK SC); `mgba_audio_low_pass_filter = "disabled"`; `mgba_color_correction = "Auto"` (v3.2 enum fix — now actually applies); Run Ahead. LCD look via `handheld/lcd-grid-v2.slangp` per-core |
| Snes9x | SNES | 1 | 2 | 1 | Integer overscale 224p; `snes9x_reduce_sprite_flicker = "enabled"`; Run Ahead |
| Mupen64Plus-Next | Nintendo 64 | 2 | 8 | 7 | No JIT; `mupen64plus-cpucore = "cached_interpreter"`; `mupen64plus-rdp-plugin = "angrylion"` + `mupen64plus-rsp-plugin = "cxd4"` — **platform-forced on Metal build** (GLideN64 needs GL, Parallel-RDP needs Vulkan, dynarec needs JIT). `mupen64plus-43screensize = "320x240"`; `mupen64plus-pak1 = "rumble"`. `mupen64plus-angrylion-multithread = "2"` (v3.3 P-core pin; A15 is 2P+3E (binned, 1 E-core fused off); fallbacks `"3"`, `"4"`). v3.9 `mupen64plus-FrameDuping = "True"` (Switch-parity smoothing for low-end/no-JIT stacks; default `True` upstream on `HAVE_LIBNX`, `False` elsewhere — Apple TV A15 matches the Switch profile). Pins: `video_threaded = "false"` ([#14978](https://github.com/libretro/RetroArch/issues/14978)), `video_frame_delay_auto = "false"` ([#14201](https://github.com/libretro/RetroArch/issues/14201)), `rewind_enable = "false"` ([#18300](https://github.com/libretro/RetroArch/issues/18300)), `run_ahead_enabled = "false"`, `audio_latency = "64"` (v3.10; was `"96"` in v3.9 — reverted to v3.6–v3.8 value; +16 ms over global 48 is sufficient when paired with v3.9's `audio_sync = "true"` + `FrameDuping = "True"` mitigations), `audio_sync = "true"` (v3.9; was `"false"` in v3.3–v3.8 — DRC <0.5% pitch shift is imperceptible and prevents audible frame drops), `autosave_interval = "0"` |
| PCSX-ReARMed | PlayStation 1 | 2 | 8 | 6 | No JIT (`pcsx_rearmed_drc = "disabled"`); `pcsx_rearmed_psxclock = "100"`; `pcsx_rearmed_gpu_thread_rendering = "enabled"` (v3.15 fix — was `"async"` in v3.3–v3.14 which is not a valid enum value per `pcsx_rearmed_opts.h` L334-348 ({auto, disabled, enabled}); silently fell back to default `"auto"`); `audio_latency = "48"`; `video_threaded = "false"` ([#14978](https://github.com/libretro/RetroArch/issues/14978)); `run_ahead_enabled = "false"`; `rewind_enable = "false"`; integer overscale (variable 256–640). `pcsx_rearmed_frameskip_type = "auto_threshold"` + `pcsx_rearmed_frameskip_threshold = "33"` (Spyro 2/3, THPS, Tekken 3). `pcsx_rearmed_cd_readahead = "333000"` (~780 MB precache; `V(333000)` is in the dropdown enum on non-3DS/non-Vita platforms per `pcsx_rearmed_opts.h` L186-189). v3.3 adds: `autosave_interval = "0"` (prevents memcard-write stall) |

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

| File | Contents | Set via |
|------|----------|---------|
| `<core>.cfg` | Frontend settings (video, audio, latency, input) | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | Core emulation options (renderer, CPU mode, accuracy) | Quick Menu → Options |

Archive path: `config/` · Device path: `config/<core_name>/` (see §7).

Mixing the two in a single file causes silent failures — RetroArch ignores core option keys in `.cfg` files and vice versa.

## 4. Frontend Override Keys

Keys actually set in one or more shipped `.cfg` files.

| Key | Values | Purpose |
|-----|--------|---------|
| `run_ahead_enabled` | `true`, `false` | Tier 1 per-core `true`; Tier 2 (PCSX-ReARMed, Mupen64Plus-Next) explicit `false` (interpreter safety; HW-GL serialize breakage) |
| `run_ahead_secondary_instance` | `true`, `false` | FBN `true` ([#16374](https://github.com/libretro/RetroArch/issues/16374)); Tier 2 Mupen + PCSX explicit `false`; Tier 1 non-FBN cores inherit global `"false"` (v3.11 removed drift-guard pins from Beetle PCE, Genesis Plus GX, Mesen, Snes9x, mGBA) |
| `video_threaded` | `false` | Tier 2 anchor ([#14978](https://github.com/libretro/RetroArch/issues/14978) Apple-platform force-disable) |
| `audio_latency` | `48`, `64` | PCSX-ReARMed `48` (v3.6 drift-guard mirror of global 48); Mupen `64` (v3.10 revert; was `96` in v3.9, `64` v3.6–v3.8; +16 ms over global 48 is sufficient when paired with v3.9's `audio_sync = "true"` + `FrameDuping = "True"` stutter mitigations) |
| `audio_sync` | `true` | Both Tier 2 cores inherit/mirror global `true` as of v3.9. Mupen flipped `false` → `true` in v3.9 (was `false` v3.3–v3.8 — the "drop frames cleanly" rationale traded audible audio gaps for pitch rubber-band, but DRC <0.5% pitch shift is imperceptible; audio gaps always are). PCSX stays true (paired with core-level `frameskip_type = "auto_threshold"` for audio buffer pressure handling) |
| `autosave_interval` | `0` | v3.3 Tier 2 pins (Mupen + PCSX); prevents purgeable-cache stall from SRAM/memcard write every 5 min |
| `video_scale_integer` | `true` | PCSX-ReARMed only (Tier 2 explicit drift-guard mirror of global); Tier 1 cores inherit global `"true"` (v3.11 removed pins — integer overscale behavior preserved by global fallback) |
| `video_scale_integer_scaling` | `1` | All Tier 1 + PCSX; integer overscale mode at 4K. PS1 variable width may shift borders |
| `video_frame_delay_auto` | `false` | Mupen `false` ([#14201](https://github.com/libretro/RetroArch/issues/14201) N64 incompat); Tier 1 + PCSX inherit global `"true"` (v3.11 removed Tier 1 drift-guard pins; PCSX has always inherited) |
| `rewind_enable` | `false` | FBN ([#16374](https://github.com/libretro/RetroArch/issues/16374)) + Mupen ([#18300](https://github.com/libretro/RetroArch/issues/18300)) + PCSX (defensive; rewind memory cost on no-JIT A15) |

Keys intentionally *not* set per-core (inherited from global `retroarch.cfg`): `preemptive_frames_enable`, `audio_resampler_quality`, `run_ahead_hide_warnings`, `run_ahead_frames` (v3.11 — global `"2"` matches the Tier 1 intent; Tier 2 cores have `run_ahead_enabled = "false"` which makes the frame count moot). `video_shader` is not set either per-core or globally — users assign presets via RetroArch's Save Core Preset UI (see §5).

## 5. Shaders

Global `retroarch.cfg` leaves the shader pipeline gate on (`video_shader_enable = "true"`) but does **not** set a global `video_shader` preset — every core starts without a preset until one is assigned via RetroArch's Save Core Preset / Save Game Preset UI (see `retroarch-appletv4k/README.md` §8), which writes dedicated `.slangp` files rather than `.cfg` entries. None of the 8 per-core `.cfg` files set `video_shader` either. `crt-easymode.slangp` is documented in the companion as the recommended starting point (single-pass, integer-scale safe with no shader geometry, cleaner 4K phosphor mask than crt-aperture) but is not auto-loaded. mGBA users who prefer the handheld-LCD aesthetic should apply `../shaders/shaders_slang/handheld/lcd-grid-v2.slangp` via Save Core Preset.

## 6. Installation

On Apple TV, create `config/<core_name>/` directories under the RetroArch config root, then move each matching `.cfg` / `.opt` pair into its core-named directory before launch. See [Manual Install: Per-Core Override Path](#7-manual-install-per-core-override-path) for the full path reference. Tier 1 `.cfg` files enable Run Ahead explicitly per core; the companion global `retroarch.cfg` remains conservative and does not enable it globally.

The override hierarchy applies automatically — no manual loading required. After uploading, launch a game with any configured core and verify via Quick Menu → Information that the override is active.

## 7. Manual Install: Per-Core Override Path

RetroArch loads per-core overrides only from a specific directory under the RetroArch config root — **not** from this repo's `config/` folder. You must manually copy files into place after cloning.

### Apple TV / tvOS

RetroArch config root on tvOS: `config/` from the WebDAV / web-interface root (which maps to the app's `Documents/RetroArch/` sandbox container). Path references below are WebDAV-relative to match the transfer flow documented in [retroarch-appletv4k §4](https://github.com/ryanmusante/retroarch-appletv4k#4-file-transfers).

For each core, create a directory named exactly after the core (spaces included) and place both the `.cfg` and `.opt` files inside:

```
config/
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

This repository uses `vMAJOR.MINOR` (no patch component) in lockstep with its companion repository — both `retroarch-appletv4k` and `retroarch-configs` share a single MAJOR.MINOR tag that increments together on every release, regardless of which side contains the real file changes. `MAJOR` increments on incompatible structural changes (filesystem layout, breaking config schema, removed features) or cross-repo version-sync events (e.g. v3.0 re-aligned the two repos after they had evolved independently at v2.95 / v1.57). `MINOR` increments on every release — additive changes, key additions/removals, documentation syncs, and conservative defaults adjustments. Each release ships with a matching `CHANGELOG.md` entry in GNU ChangeLog style (`YYYY-MM-DD<SP><SP>Author Name`; date and author separated by two spaces). `CHANGELOG.md` retains the last 5 MINOR version entries; older entries are trimmed on each release.

## 12. License

[MIT](LICENSE)
