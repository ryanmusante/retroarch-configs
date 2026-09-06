# 5.4 - 2026-09-05

  - v5.4: completeness release - additions only. No key added, removed, or
    revalued; cfg 21, opt 18. Lockstep with companion retroarch-appletv4k
    v5.4.
  - README.md: Quick Start step 3 checks the loaded .opt values (Mupen RDP
    Plugin = angrylion, CPU Core = Cached Interpreter); Cores intro links
    retroarch-appletv4k#systems for folders, extensions and BIOS names;
    Configuration notes per-core .opt loading relies on
    `global_core_options = "false"` (RetroArch default, unset globally);
    Per-Game Overrides gains the `config/<core>/<game>.opt` path and the
    removal entries (Remove Game Options, Remove Core Overrides, Reset Core
    Options). Badge 5.3 -> 5.4.
  - Verified: game-specific options path = config/<library_name>/<content
    basename>.opt (runloop.c @v1.22.2); DEFAULT_GLOBAL_CORE_OPTIONS false;
    menu labels and Mupen option labels against msg_hash_us.h and
    libretro_core_options.h.
  - config/*.cfg: header + paired stamps v5.3 -> v5.4; bodies unchanged.
  - config/*.opt: byte-identical to v5.3.
  - Companion v5.4: retroarch.cfg header + paired stamps only; 74 keys
    unchanged.
  - CHANGELOG.md: trim v4.6 per 5-release retention; retained entries are
    now v5.0-v5.4.


# 5.3 - 2026-09-05

  - v5.3: audit release against upstream core-option sources. No key added,
    removed, or revalued; cfg 21, opt 18. Lockstep with companion
    retroarch-appletv4k v5.3.
  - README.md: Quick Start reduced to two steps (create the per-core
    directory, upload the pair into it); Layout notes the root is purgeable
    cache; override table - `video_scale_integer_scaling` default wording
    (unset globally), #14201 marked closed. Badge 5.2 -> 5.3.
  - Verified: every `.opt` key and value present in the upstream
    libretro_core_options.h of its core; every `.cfg` key present in
    RetroArch configuration.c @v1.22.2; cached_interpreter is the
    non-DYNAREC default; beetle-pce-fast-libretro#127 open, RetroArch
    #14201 closed.
  - config/*.cfg: header + paired stamps v5.2 -> v5.3; bodies unchanged.
  - config/*.opt: byte-identical to v5.2.
  - Companion v5.3: retroarch.cfg header + paired stamps, upload path
    /config/; 74 keys unchanged.
  - CHANGELOG.md: trim v4.5 per 5-release retention; retained entries are
    now v4.6-v5.3.


# 5.2 - 2026-09-05

  - v5.2: final audit release. No key added, removed, or revalued; cfg 21,
    opt 18. Lockstep with companion retroarch-appletv4k v5.2.
  - README.md: File roles - `.opt` path restored to the v4.x wording, Quick
    Menu -> Core Options (per-game: Manage Core Options -> Save Game
    Options); the v5.0 "Saved via ... Manage Core Options" cell implied a
    save entry that does not exist.
  - README.md: Configuration regains the inherited-keys line
    (`preemptive_frames_enable`, `audio_resampler_quality`,
    `run_ahead_hide_warnings`, `run_ahead_frames`).
  - README.md: Core table - Beetle systems gain "(+ CD)"; Mesen regains the
    DMC rationale (sub-1% CPU saving, DPCM-heavy titles); Mupen FrameDuping
    regains its purpose (frame cadence). Badge 5.1 -> 5.2.
  - Verified: header key counts of all 14 config files, Core table `.cfg` /
    `.opt` counts and core names match the files; every cited key / value
    matches config/* or the companion retroarch.cfg.
  - config/*.cfg: header + paired stamps v5.1 -> v5.2; bodies unchanged.
  - config/*.opt: byte-identical to v5.1.
  - Companion v5.2: retroarch.cfg header + paired stamps only; 74 keys
    unchanged.
  - CHANGELOG.md: trim v4.4 per 5-release retention; retained entries are
    now v4.5-v5.2.


# 5.1 - 2026-09-05

  - v5.1: audit release - restores v5.0 removals judged vital. No key added,
    removed, or revalued; cfg 21, opt 18. Lockstep with companion
    retroarch-appletv4k v5.1.
  - README.md: Core table - Beetle PCE Fast row regains the single-instance
    run-ahead hazard (beetle-pce-fast-libretro#127 open) and
    `pce_fast_cdspeed` with the per-game `4`; Mupen row regains
    `cached_interpreter` as a drift-guard and the angrylion thread fallbacks
    (`3`-`4`, never all threads).
  - README.md: Overclocking regains the typical per-game values
    (`mesen_overclock` for Battletoads / Recca; `snes9x_overclock_superfx`
    `200%` for Star Fox, Yoshi's Island, Doom, Stunt Race FX).
  - README.md: every cited key / value verified against config/*.cfg,
    config/*.opt and the companion retroarch.cfg. Badge 5.0 -> 5.1.
  - config/*.cfg: header + paired stamps v5.0 -> v5.1; bodies unchanged.
  - config/*.opt: byte-identical to v5.0.
  - Companion v5.1: retroarch.cfg header + paired stamps only; 74 keys
    unchanged.
  - CHANGELOG.md: trim v4.3 per 5-release retention; retained entries are
    now v4.4-v5.1.


# 5.0 - 2026-09-05

  - v5.0: MAJOR - README trimmed to vital information; sections removed, so
    inbound anchors change. No key added, removed, or revalued; cfg 21, opt
    18. Lockstep with companion retroarch-appletv4k v5.0.
  - README.md: Contents, Related and the Zip contents block removed; Supported
    Cores -> Cores. Core table, override-key table, Overclocking and the
    Per-Game example condensed. Sections: Quick Start, Cores, Layout,
    Configuration, Overclocking, Per-Game Overrides, Versioning, License.
    Badge 4.6 -> 5.0.
  - config/*.cfg: header reduced to name, version, tier, key count and
    pairing; inline rationale comments removed (the README override-key table
    holds the flip / drift-guard classification). Stamps v4.6 -> v5.0.
    Section markers and key lines byte-identical.
  - config/*.opt: header reduced likewise; inline comments removed. Section
    markers and key lines byte-identical. No version stamps.
  - CHANGELOG.md: retained entries v4.3-v4.6 condensed to changed keys, files,
    stamps and lockstep; rationale prose dropped, nothing renumbered or
    redated. Trim v4.2 per 5-release retention.
