# retroarch-configs

![version](https://img.shields.io/badge/version-1.6-blue)
![cores](https://img.shields.io/badge/cores-9-green)
![license](https://img.shields.io/badge/license-MIT-yellow)

Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for the Apple TV 4K. Designed as a companion to [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k), which provides the global `retroarch.cfg`.

Override files contain only keys that differ from the global config. Core option files contain only non-default libretro core settings. Both follow the libretro override hierarchy: global → core → content directory → per-game.

## Supported Cores

| Core | Systems | Tier | `.cfg` Keys | `.opt` Keys | CRT Shader | Notes |
|------|---------|------|-------------|-------------|------------|-------|
| Beetle PCE Fast | PC Engine / TurboGrafx-16 | 1 (Flawless) | 3 | 2 | crt-easymode | Integer overscale for 240p at 4K |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 (Flawless) | 2 | — | crt-easymode | Defaults sufficient; rewind conflicts with runahead (#16374) |
| Genesis Plus GX | Genesis / Mega Drive / Sega CD / Master System | 1 (Flawless) | 3 | 5 | crt-easymode | Nuked YM2612 for accurate FM synthesis; integer overscale for 224p |
| Mesen | NES | 1 (Flawless) | 3 | 4 | crt-easymode | Integer overscale for 224p at 4K |
| mGBA | GB / GBC / GBA | 1 (Flawless) | 3 | 3 | crt-easymode | Integer overscale for GBA 240×160 at 4K |
| Snes9x | SNES | 1 (Flawless) | 3 | 2 | crt-easymode | Integer overscale for 224p at 4K |
| Mupen64Plus-Next | Nintendo 64 | 2 (Good) | 7 | 11 | zfast_crt | No JIT on tvOS; cached interpreter; run-ahead disabled by default |
| PCSX-ReARMed | PlayStation 1 | 2 (Good) | 7 | 7 | zfast_crt | No JIT on tvOS; run-ahead disabled by default |
| melonDS DS | Nintendo DS | 2 (Good) | 7 | 5 | None | Software renderer; top-screen only; shaders disabled |

**Tier definitions:** Tier 1 cores run flawlessly with 2-frame preemptive latency reduction on the Apple TV 4K. Tier 2 cores require tuning (disabled JIT, relaxed latency) and may need per-game overrides for heavier titles.

## File Structure

```
config/
├── Beetle PCE Fast/
│   ├── Beetle PCE Fast.cfg     # RetroArch frontend overrides
│   └── Beetle PCE Fast.opt     # Core options
├── FinalBurn Neo/
│   └── FinalBurn Neo.cfg       # No .opt — defaults sufficient
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

**17 files** — 9 `.cfg` (frontend overrides) + 8 `.opt` (core options).

## File Separation

RetroArch loads overrides and core options from separate files with distinct purposes.

| File | Path | Contents | Set via |
|------|------|----------|---------|
| `<core>.cfg` | `config/<core>/` | RetroArch frontend settings (video, audio, latency, input) | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | `config/<core>/` | Core-specific emulation options (renderer, CPU mode, accuracy) | Quick Menu → Options |

Mixing the two in a single file causes silent failures — RetroArch ignores core option keys in `.cfg` files and vice versa.

## Frontend Override Keys

Keys used across `.cfg` files and their purpose.

| Key | Values Used | Purpose |
|-----|-------------|---------|
| `run_ahead_frames` | `0`, `2` | Tier 1: 2-frame preemptive for reduced input lag; Tier 2: disabled (0) without JIT |
| `preemptive_frames_enable` | `false` | Disabled for interpreter-bound cores (N64, PS1, DS) |
| `video_frame_delay_auto` | `false` | Disabled for cores incompatible with auto frame delay |
| `video_threaded` | `true` | Threaded video for interpreter-bound cores (global = false) |
| `audio_latency` | `48`, `64` | Relaxed latency for interpreter-bound cores |
| `audio_resampler_quality` | `2` | Lower resampler for Tier 2 cores (global = 3) |
| `video_scale_integer_scaling` | `1` | Overscale for 224p/240p content (NES, SNES, Genesis, PCE, GBA) at 4K |
| `video_shader` | path | Per-core CRT shader preset assignment |
| `video_shader_enable` | `false` | Disable shaders where CRT is unsuitable (NDS) |

## CRT Shaders

Each core has a CRT shader assigned based on available GPU headroom on the passively-cooled A15. Only scanline-only shaders are used to avoid conflicts with the global `video_scale_integer = true` setting. Geometry shaders (crt-geom, crt-hyllian) require `video_scale_integer = false` per-core and are not assigned by default.

| Tier | Shader | GPU Cost | Cores |
|------|--------|----------|-------|
| 1 (Flawless) | `crt-easymode.slangp` | Low | Beetle PCE Fast, FinalBurn Neo, Genesis Plus GX, Mesen, mGBA, Snes9x |
| 2 (Good) | `zfast_crt.slangp` | Minimal | Mupen64Plus-Next, PCSX-ReARMed |
| 2 (Good) | None | — | melonDS DS (CRT unsuitable for NDS pixel art at 256×192) |

Shader paths use the `:/shaders_slang/crt/` prefix for tvOS. If shaders fail to load, verify the path matches your installation or apply shaders manually via Quick Menu → Shaders → Load Preset → Save Core Preset.

## Installation

Upload the `config/` directory tree to your RetroArch config path. On Apple TV, this is accessible via the web interface or WebDAV at the `config/` directory root.

The override hierarchy applies automatically — no manual loading required. After uploading, launch a game with any configured core and verify via Quick Menu → Information that the override is active.

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

Core option keys were verified against upstream sources where possible.

| Core | Key Source |
|------|-----------|
| melonDS DS | `JesseTG/melonds-ds` `constants.hpp` — prefix `melonds_` (not `melondsds_`) |
| Mupen64Plus-Next | libretro core options — prefix `mupen64plus-` |
| PCSX-ReARMed | libretro core options — prefix `pcsx_rearmed_` |
| All others | Standard libretro naming conventions |

## Related

- [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k) — Global `retroarch.cfg` and Apple TV 4K setup guide

## License

[MIT](LICENSE)
