# retroarch-configs

![version](https://img.shields.io/badge/version-1.0-blue)
![cores](https://img.shields.io/badge/cores-9-green)
![license](https://img.shields.io/badge/license-MIT-yellow)

Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for the Apple TV 4K. Designed as a companion to [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k), which provides the global `retroarch.cfg`.

Override files contain only keys that differ from the global config. Core option files contain only non-default libretro core settings. Both follow the libretro override hierarchy: global → core → content directory → per-game.

## Supported Cores

| Core | Systems | Tier | `.cfg` Keys | `.opt` Keys | Notes |
|------|---------|------|-------------|-------------|-------|
| Beetle PCE Fast | PC Engine / TurboGrafx-16 | 1 (Flawless) | 2 | 2 | |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 (Flawless) | 2 | — | Defaults sufficient; rewind conflicts with runahead (#16374) |
| Genesis Plus GX | Genesis / Mega Drive / Sega CD / Master System | 1 (Flawless) | 2 | 5 | Nuked YM2612 for accurate FM synthesis |
| Mesen | NES | 1 (Flawless) | 3 | 4 | Integer overscale for 224p at 4K |
| mGBA | GB / GBC / GBA | 1 (Flawless) | 2 | 3 | |
| Snes9x | SNES | 1 (Flawless) | 3 | 2 | Integer overscale for 224p at 4K |
| Mupen64Plus-Next | Nintendo 64 | 2 (Good) | 6 | 11 | No JIT on tvOS; cached interpreter |
| PCSX-ReARMed | PlayStation 1 | 2 (Good) | 5 | 7 | No JIT on tvOS; run-ahead disabled by default |
| melonDS DS | Nintendo DS | 2 (Good) | 6 | 4 | Software renderer; top-screen only |

**Tier definitions:** Tier 1 cores run flawlessly with runahead on the Apple TV 4K. Tier 2 cores require tuning (disabled JIT, relaxed latency) and may need per-game overrides for heavier titles.

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
| `run_ahead_frames` | `0`, `1` | Latency reduction; 0 for heavy cores without JIT |
| `rewind_enable` | `false` | Disabled in all cores; conflicts with runahead |
| `preemptive_frames_enable` | `false` | Disabled for interpreter-bound cores (N64, PS1, DS) |
| `video_frame_delay_auto` | `false` | Disabled for cores incompatible with auto frame delay |
| `video_threaded` | `true` | Threaded video for interpreter-bound cores |
| `audio_latency` | `48`, `64` | Relaxed latency for interpreter-bound cores |
| `video_scale_integer_scaling` | `1` | Overscale for 224p content (NES, SNES) at 4K |

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
