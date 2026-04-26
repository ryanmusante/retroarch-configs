2026-04-25  Ryan Musante

- v3.23: paired README de-versioning pass; 0 file-content changes.
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.22 -> v3.23 (7 surviving files; bodies byte-identical
    to v3.22).
  * config/Mupen64Plus-Next.cfg: header drops "(v3.3 stutter
    mitigations)" inline historical annotation; key body
    byte-identical.
  * config/Mupen64Plus-Next.opt: header drops "(v3.3 Angrylion-MT
    heterogeneous-ARM tune)" and "(v3.21 P2-P4 parity for Mario Kart
    64, Mario Party 1-3, GoldenEye, Perfect Dark, ProAm 64)" inline
    historical annotations; pak2/3/4 inline comment drops "v3.21:"
    prefix; key body byte-identical.
  * config/*.opt (other 6): unchanged.
  * README.md: §1 supported cores table -- Genesis Plus GX row drops
    "(v3.1 off; v3.2 enum fix)" annotation on `audio_filter`; mGBA
    row drops "(v3.9 enum fix -- prior releases used 'disabled' which
    is the UI label, not a valid key; core fell through to default
    'OFF' silently. Same class of bug as the v3.2 `color_correction`
    fix...)" and "(v3.2 enum fix -- now actually applies)" annotations;
    Mupen row drops "v3.21" / "v3.3" / "v3.9" annotations.
  * README.md: §4 Frontend Override Keys table de-versioned across
    5 row Notes columns (`run_ahead_secondary_instance`,
    `audio_latency`, `audio_sync`, `autosave_interval`,
    `video_frame_delay_auto`) plus the closing inherited-keys
    paragraph (drops "v3.11" on `run_ahead_frames`).
  * README.md: §11 Versioning drops historical re-alignment example
    "(e.g. v3.0 re-aligned the two repos after they had evolved
    independently at v2.95 / v1.57)".
  * README.md: badge 3.22 -> 3.23.
  * CHANGELOG.md: trim v3.18 entry per 5-release retention; retained
    entries are now v3.19-v3.23.
  * Companion v3.23: retroarch.cfg byte-identical to v3.22 except
    header stamp (73 keys unchanged). README §7 Additional settings
    preamble + table de-versioned across 14 rows; §7 Video table
    de-versioned across 5 rows; §9 Mupen Tier 2 row de-versioned;
    §10 Known Issues #4 de-versioned; §13 Versioning drops historical
    re-alignment example. Editorial rule established: README narrative
    no longer carries per-version history; CHANGELOG is the sole
    record of what changed when.
  * cfg 22, opt 25, cfg+opt 47 -- unchanged.

2026-04-25  Ryan Musante

- v3.22: PlayStation 1 / PCSX-ReARMed core retired; cores 8 -> 7.
  cfg+opt 61 -> 47.
  * config/PCSX-ReARMed.cfg deleted (-8 keys).
  * config/PCSX-ReARMed.opt deleted (-6 keys).
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.21 -> v3.22 (7 surviving files; bodies byte-identical
    to v3.21).
  * config/*.opt: unchanged (no version stamps; frontend-version-
    independent per v3.12 design; 7 surviving files).
  * README.md: cores badge 8 -> 7; version badge 3.21 -> 3.22.
  * README.md: §1 supported cores table drops PCSX-ReARMed row;
    Mupen64Plus-Next row already updated v3.21 for `pak1/2/3/4 =
    "rumble"` 4P parity (opt count 7 -> 10 retained).
  * README.md: §2 file structure tree drops PCSX-ReARMed.cfg/.opt
    entries; ship-count line "8 `.cfg` and 8 `.opt`" -> "7 `.cfg`
    and 7 `.opt`".
  * README.md: §4 Frontend Override Keys table de-PCSX'd across
    9 rows -- `run_ahead_enabled`, `run_ahead_secondary_instance`,
    `audio_latency` (now Mupen-only `64`), `audio_sync` (now
    Mupen-only mirror of global), `autosave_interval` (now Mupen-only
    pin), `video_scale_integer_scaling` (drops "+ PCSX" and PS1
    variable-width caveat), `video_frame_delay_auto` (drops
    "+ PCSX inherits"), `rewind_enable` (drops PCSX from list).
    `video_scale_integer` row dropped entirely -- was
    PCSX-ReARMed-only drift-guard mirror; no surviving core needs it.
  * README.md: §7 install tree drops PCSX-ReARMed/ directory block.
  * README.md: §8 Overclocking drops the `pcsx_rearmed_psxclock`
    "exception" paragraph entirely; remaining text covers
    `mesen_overclock_rate` + `snes9x_overclock` per-game guidance.
  * CHANGELOG.md: trim v3.16 + v3.17 entries; retained entries are
    now v3.18-v3.22 (5-release retention).
  * Companion v3.22: retroarch.cfg byte-identical to v3.21 except
    header stamp (73 keys unchanged); README §1 BIOS list, §4
    filesystem layout, §5 ROM/BIOS tables, §7 video/audio rows, §8
    shader recommendations table (9 -> 3 primary CRT shaders), §9
    Tier 2 supported systems table all de-PCSX'd; §8 Recommended
    presets retains `crt-easymode` + `crt-aperture` + `crt-geom`
    only.
  * cfg 22, opt 25, cfg+opt 47.

2026-04-25  Ryan Musante

- v3.21: paired Mupen 4P rumble parity; opt 28 -> 31 (Mupen +3 keys).
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.20 -> v3.21 (8 files; bodies byte-identical to v3.20).
  * config/Mupen64Plus-Next.opt: 7 -> 10 keys -- add
    `mupen64plus-pak2 = "rumble"`, `mupen64plus-pak3 = "rumble"`,
    `mupen64plus-pak4 = "rumble"` for 4P N64 rumble parity
    (Mario Kart 64, Mario Party 1-3, GoldenEye, Perfect Dark,
    ProAm 64). pak1-4 enum {rumble, memory, transfer, none} parsed
    identically per `mupen64plus-libretro-nx libretro.c` L757-820
    `update_controllers()` (rumble -> PLUGIN_RAW). Per-game overrides
    retain pak swap (e.g. Pokémon Stadium needs transfer pak).
    Header "7 keys" -> "10 keys" with v3.21 4P parity rationale.
  * config/*.opt: 7 of 8 unchanged; only `Mupen64Plus-Next.opt`
    above.
  * README.md: badge 3.20 -> 3.21; §1 Mupen `.opt` count 7 -> 10
    with 4P rumble note.
  * Companion v3.21: retroarch.cfg `menu_pause_libretro = "false"`
    -> `"true"` (reverts to upstream default; closes longest-standing
    MED finding from v3.18 audit -- Run Ahead operates on gameplay
    frames, FF is held hotkey, neither benefits from menu non-pause;
    pausing reduces thermal load on passive A15 + clean audio +
    deterministic save-state at paused frame). retroarch.cfg
    restores 3 of 4 v3.20-removed DiD pins (`network_cmd_enable`,
    `network_remote_enable`, `netplay_use_mitm_server`) as
    drift-guards on real UDP/relay attack surfaces; platform-absent
    surfaces from v3.19 (`stdin_cmd_enable`, `camera_allow`,
    `location_allow`) and `discord_allow` from v3.20 stay removed.
    70 -> 73 keys.
  * cfg 30, opt 28 -> 31, cfg+opt 58 -> 61.

2026-04-25  Ryan Musante

- v3.20: paired stamp bump; 0 file-content changes (this side).
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.19 -> v3.20 (8 files; bodies byte-identical to v3.19).
  * config/*.opt: unchanged (no version stamps; frontend-version-
    independent per v3.12 design; 8 files).
  * README.md: badge 3.19 -> 3.20.
  * Companion v3.20: retroarch.cfg drops 4 more DiD drift-guard pins
    (`network_cmd_enable`, `network_remote_enable`,
    `netplay_use_mitm_server`, `discord_allow`); 74 -> 70 keys
    (cumulative -7 since v3.18). All 4 were upstream-default `false`;
    surviving Security/Netplay block: 4 keys (2 active hardening
    flips + 2 functional pins). Driver=null block (6 keys) untouched
    -- those actively skip driver init paths.
  * cfg 30, opt 28, cfg+opt 58 -- unchanged.

2026-04-25  Ryan Musante

- v3.19: paired stamp bump; 0 file-content changes (this side).
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.18 -> v3.19 (8 files; bodies byte-identical to v3.18).
  * config/*.opt: unchanged (no version stamps; frontend-version-
    independent per v3.12 design; 8 files).
  * README.md: badge 3.18 -> 3.19.
  * CHANGELOG.md: trim v3.14 entry per 5-release retention; retained
    entries are now v3.15-v3.19.
  * Companion v3.19: retroarch.cfg removes 3 redundant defense-in-depth
    drift-guard pins (`stdin_cmd_enable`, `camera_allow`, `location_allow`
    -- all upstream defaults already `false`, all paired with
    platform-absent surfaces or `null` driver pins, all triple-redundant
    on Apple TV 4K); 77 -> 74 keys; README §7 Security row "stdin
    Command, Camera, Location" dropped entirely with v3.19 disclosure
    note in the table preamble.
  * cfg 30, opt 28, cfg+opt 58 -- unchanged.

