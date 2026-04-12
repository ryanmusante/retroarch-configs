# retroarch-configs

![version](https://img.shields.io/badge/version-1.9.1-blue)
![cores](https://img.shields.io/badge/cores-9-green)
![license](https://img.shields.io/badge/license-MIT-green)

Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for the Apple TV 4K. Designed as a companion to [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k), which provides the global `retroarch.cfg`.

Override files contain only keys that differ from the global config. Core option files contain only non-default libretro core settings. Both follow the libretro override hierarchy: global → core → content directory → per-game.

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
- [Core Option Key Verification](#core-option-key-verification)
- [Related](#related)
- [License](#license)

## Supported Cores

| Core | Systems | Tier | `.cfg` Keys | `.opt` Keys | CRT Shader | Notes |
|------|---------|------|-------------|-------------|------------|-------|
| Beetle PCE Fast | PC Engine / TurboGrafx-16 | 1 (Flawless) | 3 | 2 | crt-easymode | Integer overscale for 240p at 4K |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 (Flawless) | 2 | — | crt-easymode | Defaults sufficient; rewind conflicts with runahead (#16374) |
| Genesis Plus GX | Genesis / Mega Drive / Sega CD / Master System | 1 (Flawless) | 3 | 6 | crt-easymode | Nuked YM2612 for accurate FM synthesis; integer overscale for 224p; per-game BRAM isolation for Sega CD |
| Mesen | NES | 1 (Flawless) | 3 | 2 | crt-easymode | Integer overscale for 224p at 4K |
| mGBA | GB / GBC / GBA | 1 (Flawless) | 3 | 3 | crt-easymode | Integer overscale for GBA 240×160 at 4K |
| Snes9x | SNES | 1 (Flawless) | 3 | 1 | crt-easymode | Integer overscale for 224p at 4K |
| Mupen64Plus-Next | Nintendo 64 | 2 (Good) | 7 | 11 | zfast_crt | No JIT on tvOS; cached interpreter; run-ahead disabled by default |
| PCSX-ReARMed | PlayStation 1 | 2 (Good) | 7 | 4 | zfast_crt | No JIT on tvOS; run-ahead disabled by default |
| melonDS DS | Nintendo DS | 3 (Compromised) | 7 | 5 | None | Software 1×; top-screen only; no touchscreen on tvOS — touch-required titles unplayable |

**Tier definitions:**
- **Tier 1 (Flawless)** — runs at full speed with 2-frame run-ahead latency reduction on the Apple TV 4K.
- **Tier 2 (Good)** — requires tuning (disabled JIT, relaxed latency) and may need per-game overrides for heavier titles.
- **Tier 3 (Compromised)** — playable with significant limitations (missing input methods, low-resolution renderers, or partial compatibility). Listed for completeness; expect a degraded experience.

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
├── melonDS DS.cfg
├── melonDS DS.opt
├── Mupen64Plus-Next.cfg
├── Mupen64Plus-Next.opt
├── PCSX-ReARMed.cfg
├── PCSX-ReARMed.opt
├── Snes9x.cfg
└── Snes9x.opt
```

**17 files** — 9 `.cfg` (frontend overrides) + 8 `.opt` (core options).

For RetroArch auto-loading on Apple TV, place them under `config/<core>/` on the device:

```
config/
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
├── mGBA/
│   ├── mGBA.cfg
│   └── mGBA.opt
├── melonDS DS/
│   ├── melonDS DS.cfg
│   └── melonDS DS.opt
├── Mupen64Plus-Next/
│   ├── Mupen64Plus-Next.cfg
│   └── Mupen64Plus-Next.opt
├── PCSX-ReARMed/
│   ├── PCSX-ReARMed.cfg
│   └── PCSX-ReARMed.opt
└── Snes9x/
    ├── Snes9x.cfg
    └── Snes9x.opt
```

README.md is the authoritative layout reference. The `.cfg` and `.opt` files intentionally omit layout/path notes.

## File Separation

RetroArch loads overrides and core options from separate files with distinct purposes.

| File | Path | Contents | Set via |
|------|------|----------|---------|
| `<core>.cfg` | Archive: `config/`  ·  Device: `config/<core>/` | RetroArch frontend settings (video, audio, latency, input) | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | Archive: `config/`  ·  Device: `config/<core>/` | Core-specific emulation options (renderer, CPU mode, accuracy) | Quick Menu → Options |

Mixing the two in a single file causes silent failures — RetroArch ignores core option keys in `.cfg` files and vice versa.

## Frontend Override Keys

Keys used across `.cfg` files and their purpose.

| Key | Values Used | Purpose |
|-----|-------------|---------|
| `run_ahead_frames` | `0`, `2` | Tier 1: 2-frame run-ahead for reduced input lag; Tier 2/3: disabled (0) without JIT |
| `preemptive_frames_enable` | `false` | Disabled for interpreter-bound cores (N64, PS1, DS) |
| `video_frame_delay_auto` | `false` | Disabled for cores incompatible with auto frame delay |
| `video_threaded` | `true` | Threaded video for interpreter-bound cores (global = false) |
| `audio_latency` | `48`, `64` | Per-core: 64 ms for N64 (heavier interpreter), 48 ms for PS1/DS (minimum stable) |
| `audio_resampler_quality` | `2` | Lower resampler for Tier 2/3 cores (global = 3) |
| `video_scale_integer_scaling` | `1` | Overscale for 224p/240p content (NES, SNES, Genesis, PCE, GBA) at 4K |
| `video_shader` | path | Per-core CRT shader preset assignment |
| `video_shader_enable` | `false` | Disable shaders where CRT is unsuitable (NDS) |

## CRT Shaders

Each core has a CRT shader assigned based on available GPU headroom on the passively-cooled A15. Only scanline-only shaders are used to avoid conflicts with the global `video_scale_integer = true` setting. Geometry shaders (crt-geom, crt-hyllian) require `video_scale_integer = false` per-core and are not assigned by default.

| Tier | Shader | GPU Cost | Cores |
|------|--------|----------|-------|
| 1 (Flawless) | `crt-easymode.slangp` | Low | Beetle PCE Fast, FinalBurn Neo, Genesis Plus GX, Mesen, mGBA, Snes9x |
| 2 (Good) | `zfast_crt.slangp` | Minimal | Mupen64Plus-Next, PCSX-ReARMed |
| 3 (Compromised) | None | — | melonDS DS (CRT unsuitable for NDS pixel art at 256×192) |

Shader paths use the `shaders_slang/crt/` path for tvOS. If shaders fail to load, verify the path matches your installation or apply shaders manually via Quick Menu → Shaders → Load Preset → Save Core Preset.

## Installation

> **Note:** The ZIP ships all `.cfg` and `.opt` files flat in `config/`. That is intentional. README.md is the authoritative source for the target on-device layout.

On Apple TV, create `config/<core>/` directories, then move each matching `.cfg` / `.opt` pair into its core-named directory before launch.

The override hierarchy applies automatically — no manual loading required. After uploading, launch a game with any configured core and verify via Quick Menu → Information that the override is active.

## Overclocking

Overclock keys are intentionally not set in any core-level `.opt` file. They are per-game-only because incorrect overclocking breaks timing-sensitive titles. Apply them via per-game `.opt` overrides.

### Mesen (NES CPU)

`mesen_overclock` reduces slowdown in CPU-bound NES titles. Use `Before NMI (Recommended)` insertion point — it is the safest and avoids breaking timing.

`config/Mesen/Battletoads (USA).opt`:
```
mesen_overclock = "Medium"
mesen_overclock_type = "Before NMI (Recommended)"
```

Known beneficiaries: Battletoads, Probotector / Contra Force, Recca. Valid `mesen_overclock` values: `None`, `Low`, `Medium`, `High`, `Very High`.

### Snes9x (SNES main CPU)

`snes9x_overclock_cycles` reduces SNES main-CPU slowdown. Valid values: `disabled`, `light`, `compatible`, `max`.

`config/Snes9x/Gradius III (USA).opt`:
```
snes9x_overclock_cycles = "light"
```

### Snes9x (SuperFX)

`snes9x_overclock` is the SuperFX coprocessor frequency multiplier. Valid values: `50%` through `500%` in 10% steps; default `100%`. Used for Star Fox, Yoshi's Island, Doom, Stunt Race FX.

`config/Snes9x/Star Fox (USA).opt`:
```
snes9x_overclock = "200%"
```

`config/Snes9x/Star Fox (USA).cfg`:
```
run_ahead_frames = "1"
```

## Per-Game Overrides

For titles that need individual tuning, create per-game files in the same `config/<core>/` directory.

**Example — enabling runahead for Super Mario 64:**

`config/Mupen64Plus-Next/Super Mario 64 (USA).cfg`:
```
run_ahead_frames = "1"
```

**Example — lowering PSX clock for Vagrant Story:**

`config/PCSX-ReARMed/Vagrant Story (USA).opt`:
```
pcsx_rearmed_psxclock = "50"
```


## Core Option Key Verification

All core option keys in `.opt` files are verified against upstream sources. Every key listed below was grep-verified to exist in the linked source at release time.

| Core | Upstream Source | Prefix |
|------|-----------------|--------|
| Beetle PCE Fast | `libretro/beetle-pce-fast-libretro/libretro_core_options.h` | `pce_fast_` |
| FinalBurn Neo | No `.opt` file — defaults sufficient | `fbneo_` |
| Genesis Plus GX | `libretro/Genesis-Plus-GX/libretro/libretro_core_options.h` | `genesis_plus_gx_` |
| Mesen | `libretro/Mesen/Libretro/libretro_core_options.h` | `mesen_` |
| mGBA | `libretro/mgba/src/platform/libretro/libretro_core_options.h` | `mgba_` |
| Mupen64Plus-Next | `libretro/mupen64plus-libretro-nx/libretro/libretro_core_options.h` (CORE_NAME = `mupen64plus`, per `custom/mupen64plus-next_common.h`) | `mupen64plus-` |
| PCSX-ReARMed | `libretro/pcsx_rearmed/frontend/libretro.c` + `libretro_core_options.h` | `pcsx_rearmed_` |
| Snes9x | `libretro/snes9x/libretro/libretro_core_options.h` | `snes9x_` |
| melonDS DS | `JesseTG/melonds-ds/src/libretro/config/constants.hpp` | `melonds_` (not `melondsds_`) |

**Keys that do NOT exist in upstream and are silently ignored** (removed in v1.9 — see CHANGELOG):
- `genesis_plus_gx_bram` — real keys are split: `genesis_plus_gx_system_bram` and `genesis_plus_gx_cart_bram`
- `pcsx_rearmed_duping_enable` — no such option; frame duping is automatic in PCSX-ReARMed
- `pcsx_rearmed_async_cd` — no such option; CD async is now internal (`cdrom-async.h`). User-facing knob is `pcsx_rearmed_cd_readahead`

Upstream issue references (verified 2026-04-06): libretro/RetroArch#14201, #18300, #16374.

## Related

- [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k) — Global `retroarch.cfg` and Apple TV 4K setup guide

## License

[MIT](LICENSE)
