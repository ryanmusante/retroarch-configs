# retroarch-configs

[![version](https://img.shields.io/badge/version-5.0-blue.svg)](CHANGELOG.md)
[![companion](https://img.shields.io/badge/companion-retroarch--appletv4k-blue.svg)](https://github.com/ryanmusante/retroarch-appletv4k)

> Per-core RetroArch overrides (`.cfg`) and core options (`.opt`) for
> Apple TV 4K 3rd Gen (tvOS 26, RetroArch v1.22.x). Companion to
> [retroarch-appletv4k](https://github.com/ryanmusante/retroarch-appletv4k),
> which ships the global `retroarch.cfg` and the setup guide.

## Quick Start

1. Upload the 14 files in `config/` via the web interface or WebDAV.
2. On the Apple TV, create `config/<core_name>/` per core and move each `.cfg` / `.opt` pair into it ([Layout](#layout)).
3. Load content and check Quick Menu → Overrides → Active Override File.

> [!IMPORTANT]
> Overrides outside the per-core path are ignored: the global
> `run_ahead_enabled = "false"` then wins and Tier 1 run-ahead stays off.

## Cores

Tier 1 = full speed with shaders; Tier 2 = most titles at full speed.
Run Ahead uses the global `run_ahead_frames = "2"`, single instance.
Counts are keys per file.

<details open>
<summary><b>Core table</b></summary>

| Core | Systems | Tier | `.cfg` | `.opt` | Sets |
|------|---------|------|--------|--------|------|
| Beetle PCE Fast | PC Engine / TG-16 | 1 | 2 | 2 | Run Ahead; integer overscale; 2× CD speed; no sprite limit. CD-image precache is per-game only (`pce_fast_cdimagecache`) |
| FinalBurn Neo | Neo Geo / Arcade (CPS1/2/3) | 1 | 3 | 0 | Run Ahead; integer overscale; rewind off (drift-guard). Dipswitches and cheats are per-game only |
| Genesis Plus GX | Genesis / MD / Sega CD / SMS | 1 | 2 | 3 | Run Ahead; integer overscale; no sprite limit; per-game BRAM (system + cart) |
| Mesen | NES | 1 | 2 | 2 | Run Ahead; integer overscale; no sprite limit; DMC popping correction off (restore per-game) |
| mGBA | GB / GBC / GBA | 1 | 2 | 1 | Run Ahead; integer overscale; `mgba_color_correction = "Auto"` |
| Snes9x | SNES | 1 | 2 | 1 | Run Ahead; integer overscale; reduce sprite flicker |
| Mupen64Plus-Next | Nintendo 64 | 2 | 8 | 9 | angrylion RDP + cxd4 RSP + cached interpreter (no JIT); ParaLLEl-RDP/RSP and GLideN64 are inactive under the companion's `video_driver = "metal"`. angrylion threads `2`; FrameDuping; pak1–4 rumble (memory / transfer pak per-game). Frontend pins in [Configuration](#configuration) |

</details>

## Layout

RetroArch reads per-core overrides from `config/<core_name>/` under its
config root, which the tvOS web interface / WebDAV expose as `/`. The
zip ships the files flat under `config/`; on the device each pair lives
in a directory named exactly after the core, spaces included:

```
config/Mesen/
├── Mesen.cfg
└── Mesen.opt
```

## Configuration

<details open>
<summary><b>File roles</b></summary>

| File | Holds | Saved via |
|------|-------|-----------|
| `<core>.cfg` | Frontend overrides (video, audio, latency, input); version-stamped | Quick Menu → Overrides → Save Core Overrides |
| `<core>.opt` | Core options (renderer, CPU mode, accuracy); no version stamp | Quick Menu → Core Options → Manage Core Options |

</details>

> [!WARNING]
> RetroArch silently ignores core option keys in a `.cfg` and frontend
> keys in an `.opt` — never mix them.

Of the 21 `.cfg` keys, 14 are real flips against the global value and 7
are drift-guards pinned to the value they already inherit, so a change
to the global `retroarch.cfg` cannot silently move a pinned core. No
`.cfg` sets a shader preset — see
[retroarch-appletv4k#shaders](https://github.com/ryanmusante/retroarch-appletv4k#shaders).

<details open>
<summary><b>Frontend override keys</b></summary>

| Key | Value | Cores | Type |
|-----|-------|-------|------|
| `video_scale_integer_scaling` | `1` | Tier 1 | Flip — overscale (global `0`, underscale); needs global `video_scale_integer = "true"` |
| `run_ahead_enabled` | `true` | Tier 1 | Flip |
| `run_ahead_enabled` | `false` | Mupen | Drift-guard — per-frame savestate cost too high on the sw-RDP stack; opt in per-game |
| `run_ahead_secondary_instance` | `false` | Mupen | Drift-guard |
| `video_threaded` | `false` | Mupen | Drift-guard |
| `video_frame_delay_auto` | `false` | Mupen | Flip (global `true`) — regression guard for [#14201](https://github.com/libretro/RetroArch/issues/14201) |
| `audio_sync` | `true` | Mupen | Drift-guard |
| `audio_latency` | `64` | Mupen | Drift-guard |
| `autosave_interval` | `0` | Mupen | Flip (global `300`) — avoids the purgeable-cache stall on SRAM write |
| `rewind_enable` | `false` | FBN, Mupen | Drift-guard |

</details>

## Overclocking

Not set globally — a clock that fixes one title breaks another. Set per
game: Quick Menu → Core Options → Manage Core Options → Save Game
Options.

<details open>
<summary><b>Keys</b></summary>

| Core | Key | Values | Default |
|------|-----|--------|---------|
| Mesen | `mesen_overclock` | `None`, `Low`, `Medium`, `High`, `Very High` | `None` |
| Snes9x | `snes9x_overclock_superfx` | `50%`–`100%` by 10%, `150%`–`500%` by 50% | `100%` |
| Snes9x | `snes9x_overclock_cycles` | `disabled`, `light`, `compatible`, `max` | `disabled` |

</details>

## Per-Game Overrides

Per-game `.cfg` files live beside the core files, e.g.
`config/Mupen64Plus-Next/Super Mario 64 (USA).cfg`:

```
run_ahead_enabled = "true"
run_ahead_frames = "1"
```

## Versioning

`vMAJOR.MINOR`, in lockstep with `retroarch-appletv4k` — one tag per
release across both repos. `MAJOR` on incompatible structural changes,
`MINOR` otherwise. `CHANGELOG.md` retains the last 5 releases.

## License

MIT © 2026 Ryan Musante
