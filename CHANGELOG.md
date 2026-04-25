2026-04-24  Ryan Musante

- v3.16: doc-correctness sync; 0 key-value changes.
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.15 -> v3.16 (8 files; bodies byte-identical to v3.15).
  * config/*.opt: unchanged (no version stamps; frontend-version-
    independent per v3.12 design; 8 files).
  * README.md: §7 path frame aligned to WebDAV-relative `config/<core>/`
    matching companion §4; tree drops `Documents/RetroArch/` prefix
    (native sandbox path that would resolve to a wrong location if
    followed literally via the WebDAV transfer flow); leading prose
    notes the WebDAV-to-sandbox mapping with cross-link to
    companion §4.
  * README.md: badge 3.15 -> 3.16.
  * CHANGELOG.md: trim v3.11 entry per 5-release retention; retained
    entries are now v3.12-v3.16.
  * Companion v3.16: retroarch.cfg byte-identical to v3.15 except
    header stamp (77 keys unchanged); README §7 Audio `audio_sync`
    row corrected (v3.9 Mupen flip to "true"; was stale "v3.3 pins
    false"); §7 Hotkeys callout `savestate_auto_load` reworded as
    upstream-default rather than configured pin; baseline header
    label v3.15 -> v3.16.
  * cfg 30, opt 28, cfg+opt 58 -- unchanged.

2026-04-24  Ryan Musante

- v3.15: upstream-correctness pass; 4 file-content fixes across 3 files.
  cfg+opt 60 -> 58.
  * config/Mupen64Plus-Next.cfg: remove `video_max_swapchain_images = "3"`;
    section header "(driver, swapchain, frame delay)" -> "(driver, frame
    delay)"; rationale comment drops swapchain clause; 9 -> 8 keys.
    Metal driver hardcodes `MAX_INFLIGHT 1` (`gfx/common/metal/
    metal_common.h` L30) and never reads `video_max_swapchain_images`;
    setting was a no-op despite documented "frame-time variance
    absorption" rationale.
  * config/PCSX-ReARMed.cfg: remove `video_max_swapchain_images = "3"`
    (same Metal-ignored reason); section header "(driver, integer
    overscale, swapchain)" -> "(driver, integer overscale)"; rationale
    comment drops swapchain clause; 9 -> 8 keys.
  * config/PCSX-ReARMed.opt: `pcsx_rearmed_gpu_thread_rendering`
    "async" -> "enabled". Per `pcsx_rearmed_opts.h` L334-348 the valid
    enum is {auto, disabled, enabled}; "async" string does not appear
    anywhere in the core source; v3.3-v3.14 setting silently fell back
    to default "auto". "enabled" makes threaded GPU rendering explicit
    (matches "async GPU" rationale in original v3.3 header comment).
  * config/PCSX-ReARMed.opt: header drops `cd_readahead` WARNING
    block. `V(333000)` is in the dropdown enum on non-3DS/non-Vita
    platforms (which includes tvOS) per `pcsx_rearmed_opts.h` L186-189;
    menu-save will not silently truncate. Header "async GPU" wording
    also updated to "enabled GPU thread".
  * config/*.cfg: bump "paired with retroarch-appletv4k" stamps
    v3.14 -> v3.15 (8 files).
  * config/*.opt: unchanged except `PCSX-ReARMed.opt` above (8 files).
  * README.md: badge 3.14 -> 3.15; §1 Mupen `.cfg` count 9 -> 8 with
    `video_max_swapchain_images` mention dropped; §1 PCSX `.cfg` count
    9 -> 8 with `gpu_thread_rendering "async" -> "enabled"` v3.15 fix
    note + cd_readahead WARNING dropped + `video_max_swapchain_images`
    mention dropped; §4 Frontend Override Keys table drops
    `video_max_swapchain_images` row entirely.
  * CHANGELOG.md: trim v3.10 entry per 5-release retention; retained
    entries are now v3.11-v3.15.
  * Companion v3.15: retroarch.cfg 80 -> 77 keys (remove
    `video_frame_rest` -- key removed in RA v1.20.0; remove
    `video_hdr_contrast` -- key never existed in v1.22.0; remove
    `video_max_swapchain_images` -- Metal driver ignores; flip 2 of 3
    `menu_xmb_animation_*` "2" -> "0" per upstream enum correctness
    -- only `move_up_down` exposes a "None" value; raise
    `audio_resampler_quality` "1" -> "2" per `audio_resampler.h`
    enum LOWEST -> LOWER).
  * cfg 30, opt 28, cfg+opt 58.

2026-04-24  Ryan Musante

- v3.14: doc format pass; kernel.org-style CHANGELOG trim.
  * config/*.cfg: bump header stamps and "paired with retroarch-appletv4k"
    stamps v3.13 -> v3.14 (8 files; bodies byte-identical to v3.13).
  * config/*.opt: unchanged (no version stamps; frontend-version-
    independent per v3.12 design; 8 files).
  * README.md: badge 3.13 -> 3.14.
  * CHANGELOG.md: rewrite all retained entries in kernel.org style
    (file-first bullets, imperative mood, trimmed prose).
  * CHANGELOG.md: trim v3.9 entry per 5-release retention; retained
    entries are now v3.10-v3.14.
  * Companion v3.14: retroarch.cfg byte-identical to v3.13 except
    header stamp; 80 keys unchanged; README landing paragraph
    tightened for GitHub style.
  * cfg 32, opt 28, cfg+opt 60 -- unchanged.

2026-04-24  Ryan Musante

- v3.13: doc-correctness pass + stamp bump; 0 key-value changes.
  * config/*.cfg: bump stamps v3.12 -> v3.13 (8 files; bodies byte-
    identical to v3.12).
  * config/*.opt: unchanged (8 files).
  * README.md: §4 Frontend Override Keys `audio_latency` row drops
    ambiguous "pre-v3.5 was tighter-than-global 64" clause (pre-v3.5
    global was also 64 per companion §7 history; cannot be tighter
    than global when equal); badge 3.12 -> 3.13.
  * CHANGELOG.md: v3.12 historical `fps_show` rationale patched
    (text-only; preserves key-swap narrative and 80-key count).
  * Companion v3.13: retroarch.cfg byte-identical to v3.12 except
    header stamp; README §7 `fps_show` row rationale corrected;
    §7 adds rows for `menu_show_sublabels` and `log_verbosity`
    (pre-v3.5 shipped, previously undocumented).
  * cfg 32, opt 28, cfg+opt 60 -- unchanged.

2026-04-24  Ryan Musante

- v3.12: SYNC + comment-style unification pass; 0 key-value changes.
  * config/*.cfg: bump "paired with retroarch-appletv4k" stamps
    v3.11 -> v3.12 (8 files); collapse multi-line file-header comment
    blocks to single-line form with `·` separators (same content, no
    line-wrap).
  * config/*.opt: collapse multi-line inline rationale blocks to
    single-line form with `;` clause separators (8 files). `PCSX-
    ReARMed.opt` WARNING block for `cd_readahead = "333000"` and
    `Mupen64Plus-Next.opt` angrylion-multithread fallback-ladder
    notes reduced to single-line; content preserved verbatim.
  * config/*.{cfg,opt}: key-value content byte-identical to v3.11
    across all 16 files.
  * README.md: badge 3.11 -> 3.12.
  * CHANGELOG.md: trim v3.7 entry per 5-release retention.
  * Companion v3.12: retroarch.cfg net-zero key swap (add
    `fps_show = "true"`, remove `netplay_start_as_spectator = "false"`);
    Netplay section header drops "spectator" mention; 4-line file
    header collapsed to single-line form.
  * cfg 32, opt 28, cfg+opt 60 -- unchanged.
