# 5.6 - 2026-09-26

  - v5.6: deep-scan release. No key added, removed, or revalued; cfg 21,
    opt 18. Lockstep with companion retroarch-appletv4k v5.6.
  - README.md: badge 5.5 -> 5.6; every claim re-verified, no content
    change.
  - Verified: supported extensions and firmware names of all 7 cores
    against libretro-core-info; Reset Core Options sits under Manage Core
    Options (menu_displaylist.c @v1.22.2); integer scaling 0 / 1 / 2 =
    Underscale / Overscale / Smart; all README links resolve;
    beetle-pce-fast-libretro#127 open.
  - config/*.cfg: header + paired stamps v5.5 -> v5.6; bodies unchanged.
  - config/*.opt: byte-identical to v5.5.
  - Companion v5.6: retroarch.cfg header + paired stamps; 74 keys
    unchanged; README foreground / VLAN server note, Tuning
    vrr_runloop_enable wording, cache-purge timing, tvOS controller
    limits, fbneo/neogeo.zip.
  - CHANGELOG.md: trim v5.1 per 5-release retention; retained entries are
    now v5.2-v5.6.


# 5.5 - 2026-09-26

  - v5.5: currency release against upstream core sources and tvOS 27. No
    key added, removed, or revalued; cfg 21, opt 18. Lockstep with
    companion retroarch-appletv4k v5.5.
  - README.md: intro target tvOS 26 -> 27; Quick Start step 3 shows the
    Mupen RDP Plugin value label Angrylion (as displayed, like Cached
    Interpreter); Core table Mupen row - GLideN64 and ParaLLEl-RDP force a
    GL / Vulkan driver in place of the companion's metal driver
    (driver_switch_enable, default on) and ParaLLEl-RSP needs JIT,
    replacing "inactive under metal"; Beetle row - the second-instance
    mode hangs loading CD images. Badge 5.4 -> 5.5.
  - config/Mupen64Plus-Next.opt: header note corrected likewise; key lines
    byte-identical.
  - config/.gitkeep: removed; config/ holds all 14 files.
  - Verified: all 18 .opt keys and values in each core's upstream
    libretro_core_options.h; 7 library_name strings match the directory
    names; tvos-arm64 Mupen build has HAVE_THR_AL, LLE and no DYNAREC
    (cached_interpreter default); HW-render driver forcing in
    video_driver.c @v1.22.2; beetle-pce-fast-libretro#127 open, RetroArch
    #14201 closed.
  - config/*.cfg: header + paired stamps v5.4 -> v5.5; bodies unchanged.
  - config/*.opt: other 6 byte-identical to v5.4.
  - Companion v5.5: retroarch.cfg header + paired stamps, target tvOS 27;
    74 keys unchanged; README lockout-recovery row, exact Online Updater
    labels, 8BitDo mode D, Guest-Dr-Venom dropped.
  - CHANGELOG.md: trim v5.0 per 5-release retention; retained entries are
    now v5.1-v5.5.


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
