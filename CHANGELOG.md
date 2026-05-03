2026-05-02  Ryan Musante

- v3.25: companion to retroarch-appletv4k v3.25 audit fix pass; trim 4
  inert/drift-guard keys across 2 `.opt` files; 7 `.cfg` paired stamps.
  * config/Genesis Plus GX.opt: 6 keys -> 3 keys. Removed:
    `genesis_plus_gx_ym2612 = "mame (ym2612)"` (matches upstream default
    at libretro/Genesis-Plus-GX `libretro_core_options.h:444`;
    drift-guard pin); `genesis_plus_gx_audio_filter = "disabled"`
    (matches upstream default at L475; misleading "off for CPU
    headroom" annotation — no headroom gained vs default);
    `genesis_plus_gx_render = "single field"` (matches upstream default
    at L347; drift-guard pin). Retained: `no_sprite_limit` (real flip),
    `system_bram` + `cart_bram` (real flips, per-game BRAM). Header
    rewritten to reflect 3 remaining keys.
  * config/Mupen64Plus-Next.opt: 10 keys -> 9 keys. Removed:
    `mupen64plus-43screensize = "320x240"` — under HAVE_THR_AL
    angrylion path, value is read at libretro/mupen64plus-libretro-nx
    `libretro/libretro.c:1348` then immediately overwritten at
    L1370-1371 by `if(current_rdp_type == RDP_PLUGIN_ANGRYLION)`
    forced override to `retro_screen_width=640, retro_screen_height=480,
    retro_screen_aspect=4.0/3.0, AspectRatio=1`. Functionally inert
    under the platform-forced `rdp-plugin=angrylion` stack. Header
    drops 43screensize reference; remaining 9 keys retain.
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.24 -> v3.25 (7 files; 6 bodies byte-identical to v3.24;
    Mupen64Plus-Next.cfg additionally trims one verbose multi-clause
    audio comment line to single-line per editorial directive).
  * config/*.opt: 3 files (Mesen / Mupen / mGBA) trim verbose
    multi-clause comments to single-line and drop legacy `(v3.X)`
    historical annotations per established v3.23 editorial rule.
    Beetle PCE Fast.opt / FinalBurn Neo.opt / Snes9x.opt unchanged
    — already minimal at v3.24.
  * README.md: §1 Supported Cores table — Genesis Plus GX `.opt` count
    6 -> 3 (notes column rewritten; drift-guard rationales removed
    where keys removed); Mupen64Plus-Next `.opt` count 10 -> 9 (notes
    column drops `43screensize` reference).
  * README.md: §2 File Structure tree unchanged (file paths same; only
    contents trimmed).
  * README.md: §4 Frontend Override Keys table unchanged (only `.cfg`
    keys; `.opt` changes don't affect this section).
  * README.md: badge 3.24 -> 3.25.
  * CHANGELOG.md: trim v3.20 entry per 5-release retention; retained
    entries are now v3.21-v3.25.
  * Companion v3.25: retroarch.cfg `audio_resampler_quality "2" -> "3"`
    (real flip from tvOS LOWER default to NORMAL; SINC vs linear; A15
    sub-1% CPU); + `input_max_users = "4"` (multi-player phantom-slot
    cap; #16685 cross-reference; 4P pak alignment); key count 73 -> 74.
    README intro "73-key" -> "74-key"; §7 Audio resampler row rewritten;
    §7 Input + new `input_max_users` row; badge 3.24 -> 3.25; CHANGELOG
    trim v3.20 per matching 5-release retention.
  * cfg 22, opt 25 -> 21, cfg+opt 47 -> 43.

2026-04-25  Ryan Musante

- v3.24: paired README §5 shader-recommendation retarget; 0 file-content
  changes.
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.23 -> v3.24 (7 files; bodies byte-identical to v3.23).
  * config/*.opt: unchanged (no version stamps; frontend-version-
    independent per v3.12 design; 7 files).
  * README.md: §5 Shaders retargets recommended-starting-point
    reference from `crt-easymode.slangp` to `crt/zfast-crt.slangp`
    (single-pass, integer-scale safe with no shader geometry,
    designed for low-end GPUs). Drops the prior "cleaner 4K phosphor
    mask than crt-aperture" comparative clause (no longer applicable
    -- crt-aperture not in current lineup). Path corrected from
    `../shaders/shaders_slang/handheld/lcd-grid-v2.slangp` to
    `handheld/lcd-grid-v2.slangp` (matches companion §8 reference
    style and RetroArch UI navigation context). mGBA LCD recommendation
    target unchanged.
  * README.md: §5 fixes stale "8 per-core `.cfg` files" -> "7 per-core
    `.cfg` files". v3.22 dropped PCSX-ReARMed core (cores 8 -> 7);
    §1 supported cores table and §2 file structure tree were updated
    in that pass but §5 prose was missed.
  * README.md: badge 3.23 -> 3.24.
  * CHANGELOG.md: strip audit reference from retained v3.21 entry
    (parenthetical clause inside `Companion v3.21` menu_pause_libretro
    bullet) per editorial directive. Substantive technical rationale
    preserved verbatim.
  * CHANGELOG.md: trim v3.19 entry per 5-release retention; retained
    entries are now v3.20-v3.24.
  * Companion v3.24: retroarch.cfg byte-identical to v3.23 except
    header stamp (73 keys unchanged). README intro stale "77-key"
    -> "73-key" fix (drift since v3.19; intro paragraph was missed
    in each subsequent README pass). README §8 Recommended presets
    table 3 -> 2 rows (`crt/zfast-crt.slangp` Minimal cost as sole
    CRT recommendation + `handheld/lcd-grid-v2.slangp` Minimal cost
    as sole mGBA-LCD recommendation; replaces `crt-easymode` /
    `crt-aperture` / `crt-geom`). §8 parameter callout retargeted
    with verified zfast-crt impl parameters (BLURSCALEX, LOWLUMSCAN,
    HILUMSCAN, BRIGHTBOOST, MASK_DARK, MASK_FADE -- sourced from
    `crt/shaders/zfast_crt/zfast_crt_impl.inc` on libretro/slang-shaders
    master). §8 "Integer Scaling Conflict" callout dropped (no longer
    relevant to single-pass / integer-scale-safe lineup; multi-pass
    shaders covered by surviving "Avoid on Apple TV" line). §8
    "Handheld note" + step 3 cross-reference + "Applying ... per-core"
    sentence retargeted from `crt-easymode` to `zfast-crt`. CHANGELOG
    audit-reference strip from retained v3.21 entry per matching
    editorial directive.
  * cfg 22, opt 25, cfg+opt 47 -- unchanged.

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
    -> `"true"` (reverts to upstream default; Run Ahead operates on
    gameplay frames, FF is held hotkey, neither benefits from menu
    non-pause; pausing reduces thermal load on passive A15 + clean
    audio + deterministic save-state at paused frame). retroarch.cfg
    restores 3 of 4 v3.20-removed DiD pins (`network_cmd_enable`,
    `network_remote_enable`, `netplay_use_mitm_server`) as
    drift-guards on real UDP/relay attack surfaces; platform-absent
    surfaces from v3.19 (`stdin_cmd_enable`, `camera_allow`,
    `location_allow`) and `discord_allow` from v3.20 stay removed.
    70 -> 73 keys.
  * cfg 30, opt 28 -> 31, cfg+opt 58 -> 61.

