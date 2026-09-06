# 4.6 - 2026-09-05

  - v4.6: README release. No key added, removed, or revalued; cfg 21, opt 18,
    cfg+opt 39 unchanged. Lockstep with companion retroarch-appletv4k v4.6.
  - README.md: all 5 `<details>` blocks now carry the `open` attribute -
    expanded by default, still collapsible. Block set and contents unchanged
    from v4.5.
  - README.md: version badge 4.5 -> 4.6.
  - config/*.cfg: 7 files; header and paired stamps v4.5 -> v4.6. Bodies
    byte-identical to v4.5.
  - config/*.opt: 7 files byte-identical to v4.5 (no version stamps).
  - Companion v4.6: retroarch.cfg header + paired stamps only; 74 keys
    unchanged.
  - CHANGELOG.md: trim v4.1 per 5-release retention; retained entries are
    now v4.2-v4.6.


# 4.5 - 2026-09-05

  - v4.5: README release. No key added, removed, or revalued; cfg 21, opt 18,
    cfg+opt 39 unchanged. Lockstep with companion retroarch-appletv4k v4.5.
  - README.md: every table now sits in a default-collapsed `<details>` block.
    New: Core table, File roles, Overclock keys; kept: Zip contents (flat),
    Frontend override keys. Prose, Quick Start steps, admonitions and the two
    code examples stay open. 5 blocks total (was 3).
  - README.md: Shaders `<details>` block removed - it duplicated the companion
    Shaders section. One sentence in Frontend override keys now links
    retroarch-appletv4k#shaders instead.
  - README.md: Supported Cores gains the companion's one-line Tier definition.
    Mupen Notes trimmed (the MoltenVK and 2P+3E detail stays in
    Mupen64Plus-Next.opt); FinalBurn Neo Notes drops the 2026-05-12 FBNeo
    README date (kept in FinalBurn Neo.cfg and the v4.3 entry). Facts
    unchanged.
  - README.md: lockstep stated once, in Versioning; intro and Related drop
    their restatements. "See CHANGELOG for release history" dropped - the
    version badge already links it.
  - README.md: version badge 4.4 -> 4.5.
  - config/*.cfg: 7 files; header and paired stamps v4.4 -> v4.5. Bodies
    byte-identical to v4.4.
  - config/*.opt: 7 files byte-identical to v4.4 (no version stamps).
  - Companion v4.5: retroarch.cfg header + paired stamps only; 74 keys
    unchanged.
  - CHANGELOG.md: trim v4.0 per 5-release retention; retained entries are
    now v4.1-v4.5.


# 4.4 - 2026-09-05

  - v4.4: audit release. No key added, removed, or revalued; cfg 21, opt 18,
    cfg+opt 39 unchanged. Lockstep with companion retroarch-appletv4k v4.4.
  - Mupen64Plus-Next.opt / README.md: the v4.3 "tvOS does not provide Vulkan"
    claim was wrong; RetroArch tvOS ships MoltenVK Vulkan, the Apple default.
  - Mupen64Plus-Next.opt / README.md: ParaLLEl-RDP/RSP and GLideN64 stay
    inactive only because the companion pins `video_driver = "metal"`.
  - Mupen64Plus-Next.cfg: `video_frame_delay_auto` comment loses the
    contradictory "drift-guard, real override" (real override, #14201 guard).
  - Mupen64Plus-Next.cfg: Video section title "(driver, frame delay)" ->
    "(threading, frame delay)"; header plugin names lower-cased.
  - Beetle PCE Fast.cfg: header names the failure #127 guards against
    (second-instance run-ahead hangs CD images; issue still open).
  - FinalBurn Neo.opt / README.md: per-game options path gains "Manage Core
    Options" (v1.22.2 tree); Mesen.opt "(max CPU)" -> "(CPU headroom)".
  - README.md: shader preset path gains "Manage Presets" (v1.22.2); Overrides
    verify step names "Active Override File"; Mupen row uses full option keys.
  - README.md: companion badge, intro link and Related entry point at the
    companion; Versioning "last 5 MINOR entries" -> "last 5 releases".
  - config/.gitkeep: v4.3 recorded its removal but the file stayed tracked;
    removed now. The Quick Start count of 14 files is unchanged.
  - config/*.cfg: 7 files; header and paired stamps v4.3 -> v4.4. Bodies
    byte-identical bar Beetle PCE Fast.cfg and Mupen64Plus-Next.cfg comments.
  - config/*.opt: no version stamps; comment-only edits to FinalBurn Neo.opt,
    Mesen.opt and Mupen64Plus-Next.opt; the other 4 byte-identical to v4.3.
  - README.md: version badge 4.3 -> 4.4.
  - Companion v4.4: retroarch.cfg header stamp and two section comments only;
    74 keys unchanged.
  - CHANGELOG.md: reflowed to kernel.org shape (ASCII, 78 columns, 2 blank
    lines between blocks; nothing reworded); trim v3.28 per retention.


# 4.3 - 2026-08-30

  - v4.3: documentation and comment correction release. No key added, removed,
    or revalued; cfg 21, opt 18, cfg+opt 39 unchanged. Released in lockstep
    with companion retroarch-appletv4k v4.3.
  - FinalBurn Neo.cfg / README.md: `rewind_enable = "false"` rationale
    corrected. Upstream FBNeo `src/burner/libretro/README.md` struck the
    #16374 note on 2026-05-12 ("this bug is seemingly fixed") and RetroArch
    #16374 is closed. The pin is value-identical to global `"false"`, so it is
    now documented as a drift-guard rather than a live workaround. Value
    unchanged.
  - README.md / Mupen64Plus-Next.cfg: closed-issue citations marked. #14978
    (`video_threaded`) and #18300 (`rewind_enable`) are closed upstream and
    both pins equal the global value; both are now labelled drift-guards,
    matching the treatment given #14201 at v4.2. #14201 itself is relabelled a
    real override retained as a regression guard, since global is `"true"` and
    Mupen pins `"false"`. Values unchanged.
  - Mupen64Plus-Next.opt: `angrylion-multithread` comment corrected. The
    option sets the angrylion worker-thread count; it does not set CPU
    affinity, and tvOS exposes no P-core pinning to applications. Reframed as
    a worker budget sized to the 5-core A15 bin (2P+3E). Value `"2"`
    unchanged.
  - Mupen64Plus-Next.cfg / .opt / README.md: "Metal-only (no GL/Vulkan)"
    reworded to "software stack". The tvOS build of mupen64plus-next does
    compile ParaLLEl-RDP and ParaLLEl-RSP (`Makefile` tvOS branch sets
    `HAVE_PARALLEL_RDP`, `HAVE_PARALLEL_RSP`, `HAVE_THR_AL`, `LLE`), so both
    appear in the core-option lists. They are unusable at runtime: RetroArch
    tvOS ships no Vulkan driver, and GLideN64 needs a GL context that
    `video_driver = "metal"` does not provide. Prior wording implied the
    plugins were absent from the build.
  - README.md: Frontend override keys - the 21 per-core keys are now split
    into 14 real flips and 7 drift-guards set to the value they already
    inherit. Drift-guards: FBN `rewind_enable`; Mupen `video_threaded`,
    `audio_sync`, `audio_latency`, `run_ahead_enabled`,
    `run_ahead_secondary_instance`, `rewind_enable`.
  - README.md: Frontend override keys - `video_scale_integer_scaling` row
    states the enum. `"1"` is overscale against upstream default `"0"`
    (underscale), and the key is inert unless global `video_scale_integer =
    "true"`.
  - README.md: Overclocking - `snes9x_overclock_superfx` range corrected. The
    value list is discrete, not continuous: 50%-100% in 10% steps, then
    150%-500% in 50% steps.
  - README.md: Configuration - note added that `.cfg` headers are
    version-stamped and `.opt` headers deliberately are not
    (frontend-version-independent per v3.12 design).
  - config/.gitkeep: removed. The directory has carried 14 tracked files since
    v3.x, so the placeholder is inert; Quick Start already states 14 files.
  - README.md: the v4.0-v4.2 entries record a `paired` cross-link badge bump.
    No paired badge is present in the file - the header carries a single
    version badge. Recorded here rather than by editing the historical
    entries.
  - config/*.cfg: 7 files; header and paired stamps v4.2 -> v4.3. Bodies
    byte-identical to v4.2 except comment text in FinalBurn Neo.cfg and
    Mupen64Plus-Next.cfg.
  - config/*.opt: no version stamps; comment-only edits to
    Mupen64Plus-Next.opt, other 6 byte-identical to v4.2.
  - README.md: version badge 4.2 -> 4.3.
  - Companion v4.3: retroarch.cfg byte-identical to v4.2 except header stamp
    (74 keys unchanged).
  - CHANGELOG.md: trim v3.27 entry per 5-release retention; retained entries
    are now v3.28 + v4.0-v4.3.


# 4.2 - 2026-07-26

  - v4.2: documentation-only release. No key added, removed, or revalued; cfg
    21, opt 18, cfg+opt 39 unchanged.
  - Mupen64Plus-Next.cfg: `audio_latency` comment corrected. Companion v4.1
    raised global `audio_latency "48" -> "64"`, so the pin no longer sits +16
    ms above global - it now equals it. Reframed as an explicit Tier 2 pin
    held against global drift, matching the treatment of `audio_sync`,
    `video_threaded`, `run_ahead_secondary_instance`, and `rewind_enable`.
    Value `"64"` unchanged.
  - README.md: Frontend override keys - `audio_latency` row rewritten for the
    same reason.
  - README.md: Layout - tvOS path mapping corrected. `Documents/RetroArch/` is
    exposed as `/` by the web interface / WebDAV, not as `config/`; per-core
    directories are therefore at `/config/<core_name>/`. Prior wording implied
    `config/config/<core_name>/`.
  - README.md / Mupen64Plus-Next.cfg: `run_ahead_enabled = "false"` rationale
    corrected. The prior "HW-GL serialize breakage" note contradicted the
    documented Metal-only, software-RDP stack and the repo's own per-game
    run-ahead example. Reframed as a per-frame savestate cost decision with
    per-game opt-in. Value unchanged.
  - README.md / FinalBurn Neo.opt: menu path corrected to Quick Menu -> Core
    Options, matching the RetroArch v1.22.0 label and the Overclocking
    section.
  - README.md: Frontend override keys - the inherited-keys note referenced
    `video_shader`, which is not a config key in RetroArch v1.22.0. Reworded
    to state that no `.cfg` sets a shader preset and that presets are assigned
    per-core via Save Core Preset.
  - Beetle PCE Fast.opt: CD comment trimmed. The 4 GB precache rationale is
    stated in the header line; the inline restatement is dropped, keeping
    `cdspeed` guidance and the per-game `cdimagecache` opt-in.
  - config/*.cfg: 7 files; header and paired stamps v4.1 -> v4.2. Bodies
    byte-identical to v4.1 except Mupen64Plus-Next.cfg (comment only).
  - config/*.opt: no version stamps (frontend-version-independent per v3.12
    design); comment-only edits to Beetle PCE Fast.opt and FinalBurn Neo.opt.
  - README.md: version badge 4.1 -> 4.2; paired badge `retroarch--appletv4k
    v4.1` -> `v4.2`.
  - Companion v4.2: retroarch.cfg byte-identical to v4.1 except header stamp
    (74 keys unchanged).
  - CHANGELOG.md: trim v3.26 entry per 5-release retention; retained entries
    are now v3.27-v3.28 + v4.0-v4.2.
