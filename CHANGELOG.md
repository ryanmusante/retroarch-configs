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


# 4.6 - 2026-09-05

  - README.md: all 5 `<details>` blocks gain `open`. Badge 4.5 -> 4.6.
  - config/*.cfg: header + paired stamps v4.5 -> v4.6; bodies unchanged. Keys
    unchanged: cfg 21, opt 18. Lockstep with retroarch-appletv4k v4.6.
    CHANGELOG: trim v4.1.


# 4.5 - 2026-09-05

  - README.md: every table in a default-collapsed `<details>` block (3 -> 5);
    Shaders block replaced by a link to retroarch-appletv4k#shaders; Tier
    definition added; Mupen / FBN notes, lockstep restatements and the
    CHANGELOG pointer trimmed. Badge 4.4 -> 4.5.
  - config/*.cfg: header + paired stamps v4.4 -> v4.5; bodies unchanged. Keys
    unchanged: cfg 21, opt 18. Lockstep with retroarch-appletv4k v4.5.
    CHANGELOG: trim v4.0.


# 4.4 - 2026-09-05

  - Mupen64Plus-Next.opt / README.md: the v4.3 "tvOS does not provide Vulkan"
    claim corrected - RetroArch tvOS ships MoltenVK Vulkan; ParaLLEl-RDP/RSP
    and GLideN64 stay inactive only because the companion pins
    `video_driver = "metal"`.
  - Comment-only edits: Beetle PCE Fast.cfg (#127 guard named),
    Mupen64Plus-Next.cfg, FinalBurn Neo.opt, Mesen.opt. README menu paths
    follow the v1.22.2 tree. config/.gitkeep removed (v4.3 recorded the
    removal; the file had stayed tracked).
  - config/*.cfg: header + paired stamps v4.3 -> v4.4. Badge 4.3 -> 4.4. Keys
    unchanged: cfg 21, opt 18. Lockstep with retroarch-appletv4k v4.4.
    CHANGELOG: kernel.org reflow; trim v3.28.


# 4.3 - 2026-08-30

  - FinalBurn Neo.cfg / README.md: `rewind_enable = "false"` reclassified as a
    drift-guard (#16374 closed). Value unchanged.
  - Mupen64Plus-Next.cfg / README.md: `video_threaded` (#14978) and
    `rewind_enable` (#18300) labelled drift-guards; `video_frame_delay_auto`
    a real override kept as #14201 regression guard. Values unchanged.
  - Mupen64Plus-Next.opt: `angrylion-multithread` documented as worker-thread
    count, not CPU affinity; "Metal-only" -> "software stack".
  - README.md: 21 per-core keys = 14 real flips + 7 drift-guards;
    `video_scale_integer_scaling` enum stated; `snes9x_overclock_superfx`
    range corrected (50%-100% by 10%, 150%-500% by 50%). Badge 4.2 -> 4.3.
  - config/*.cfg: header + paired stamps v4.2 -> v4.3. Keys unchanged: cfg
    21, opt 18. Lockstep with retroarch-appletv4k v4.3. CHANGELOG: trim v3.27.
