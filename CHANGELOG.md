2026-04-12  Ryan Musante

- v1.26: AUDIT — REV 2 audit pass against libretro/RetroArch master source. Applied 21 findings across all 15 per-core files.
- v1.26: `Mupen64Plus-Next.opt` — remove 5 dead GLideN64-only keys (`EnableCopyColorToRDRAM`, `EnableCopyColorFromRDRAM`, `EnableCopyDepthToRDRAM`, `txFilterMode`, `EnableLODEmulation`) that are silently ignored when `rdp-plugin=angrylion`. Dead config verified against mupen64plus-libretro-nx source. Key count 12 → 7.
- v1.26: `Mupen64Plus-Next.cfg` — remove 3 keys that duplicated global defaults (`run_ahead_frames=0`, `preemptive_frames_enable=false`, `video_frame_delay_auto=false`). Flip `video_threaded` true → false to match the global tvOS Metal crash #14978 defense. Key count 7 → 4.
- v1.26: `PCSX-ReARMed.cfg` — same duplicate-default cleanup and `video_threaded` true → false. Key count 7 → 4.
- v1.26: `FinalBurn Neo.cfg` — add `video_scale_integer_scaling="1"` (was the only Tier 1 core missing it; FBN outputs 224p-304p benefit from integer scale at 4K). Key count 2 → 3.
- v1.26: `Beetle PCE Fast.cfg` — drop `run_ahead_frames` 2 → 1. Beetle PCE Fast CDROM seek state is not fully deterministic under second-instance-disabled run-ahead; 1 frame is the safe ceiling.
- v1.26: `mGBA.opt` — change `mgba_interframe_blending` from `mix_smart` to `mix`. mix_smart adds shader-stage GPU cost on top of 4K integer overscale under A15 fillrate contention.
- v1.26: `Snes9x.cfg`, `Mesen.cfg` — remove orphan "scanline shader; integer-overscale-safe" comments that had no associated `video_shader` key (stale from v1.15 per-core shader removal).
- v1.26: README core summary table refreshed to match new key counts and behavior. Companion retroarch-appletv4k v2.60 ships the global-side fixes (audio_rate_control_delta, autosave_interval, savestate_max_keep, menu_throttle_framerate removal, comment repositioning).
- v1.26: README — add "Manual Install: Per-Core Override Path" section with full tvOS directory tree, verification step, and warning that without correct install path the global `run_ahead_enabled=false` wins and Tier 1 run-ahead is inert.

2026-04-12  Ryan Musante

- v1.25: STYLE — shorten every comment in all 15 per-core `.cfg` and `.opt` files without removing any. Total 22 comments tightened, 1164 bytes saved. Every comment preserved, meaning intact, just less boilerplate prose.

2026-04-12  Ryan Musante

- v1.24: README trim — remove redundant "Tier definitions" bullet list after the core summary table (B1; table already has a Tier column); replace the second ASCII diagram showing on-device `config/<core_name>/` layout with a cross-reference to `retroarch-appletv4k/README.md` §4 (B2); drop redundant "16 files" file-count line (B3); rewrite stale `## CRT Shaders` section to document the v1.23 global crt-easymode + Tier 2 zfast_crt override model in one paragraph (B6); compress `## Overclocking` from per-core subsections to a single paragraph (B7); drop duplicate per-game override example, keep one (B8); drop entire `## Core Option Key Verification` section — audit detail belongs in CHANGELOG not README (B9). TOC updated.
- v1.24: 12385 → 7827 bytes (37% size reduction), zero loss of actionable content.

2026-04-12  Ryan Musante

- v1.23: Tier 2 `.cfg` files — add explicit `video_shader = "shaders_slang/crt/zfast_crt.slangp"` to both `Mupen64Plus-Next.cfg` and `PCSX-ReARMed.cfg` as per-core overrides of the new global crt-easymode default in the companion retroarch-appletv4k v2.55. zfast_crt is the cheapest CRT preset and is the only scanline shader that fits the GPU budget alongside Angrylion software RDP (N64) or the PSX interpreter. Removes pre-existing orphan "CRT shader: minimal GPU cost" comment that had no associated key.
- v1.23: README core summary table — update CRT Shader column from "user-applied" to `crt-easymode (global)` for Tier 1 (6 rows) and `zfast_crt (override)` for Tier 2 (2 rows); bump Tier 2 `.cfg` key counts 6 → 7 to reflect the added video_shader key.

2026-04-12  Ryan Musante

- v1.22: STYLE — collapse multiline comment runs in all per-core `.cfg` and `.opt` files to single-line comments. Affected files: Beetle PCE Fast.cfg (4→2), Beetle PCE Fast.opt (2→1), FinalBurn Neo.cfg (3→1), Genesis Plus GX.cfg (4→2), Genesis Plus GX.opt (3→2), Mesen.cfg (4→3), Snes9x.cfg (4→3), mGBA.cfg (4→2). No key or value changes; comment content preserved and joined with single spaces.

2026-04-12  Ryan Musante

- v1.21: README — sync core summary table to actual file contents. Stale references corrected: Tier 1 `.cfg` key counts 4→3 and Tier 2 `.cfg` counts 7→6 (the dropped key was `video_shader`, removed in v1.15 but never reflected in the table); FinalBurn Neo `.cfg` 3→2 (same reason); Mupen64Plus-Next `.opt` 11→12 (Player 1 Rumble Pak added in v1.19).
- v1.21: README — Genesis Plus GX row note updated from "Nuked YM2612 for accurate FM synthesis" to "MAME YM2612 (thermal-safe baseline; switch to Nuked per-game for audiophile titles)" matching v1.20 perf fix.
- v1.21: README — Mupen64Plus-Next row note expanded to document Player 1 Rumble Pak default and `CopyDepthToRDRAM = Off` baseline plus the per-game `Software` exceptions (OoT/MM/Conker/Body Harvest).
- v1.21: README — PCSX-ReARMed row note expanded to document `psxclock = 100` native baseline and async GPU threading.
- v1.21: README — CRT Shader column entries changed from `crt-easymode` / `zfast_crt` (no longer pre-assigned) to `user-applied` matching the v1.15 removal of per-core `video_shader` keys.
- v1.21: no `.cfg` or `.opt` content changes; documentation-only sync.

2026-04-12  Ryan Musante

- v1.20: PERF — `Genesis Plus GX.opt` set `genesis_plus_gx_ym2612 = "mame (ym2612)"` (was `nuked (ym2612)`). Nuked YM2612 is cycle-accurate but ~3-5x the CPU cost of MAME and was the largest single CPU sink in the GPGX core under Run-Ahead 2 + CRT shader on the passively-cooled A15. MAME is the appropriate baseline for Apple TV's thermal envelope; restore Nuked per-game for audiophile titles if desired.
- v1.20: PERF — `Mupen64Plus-Next.opt` set `mupen64plus-EnableCopyDepthToRDRAM = "Off"` (was `Software`). Software depth copy under Angrylion software RDP is one of the most expensive Mupen operations and was leaving ~10-20% headroom on the table for an N64 core that is already CPU-bound on Tier 2. Re-enable per-game (`Software`) for the small set that needs it: Zelda OoT/MM (lens of truth), Body Harvest, Conker's Bad Fur Day (fog).
- v1.20: COMPAT — `PCSX-ReARMed.opt` set `pcsx_rearmed_psxclock = "100"` (was `57`). The previous global `57` underclock was below the safe-compatibility floor (~80) and broke timing in many titles (audio cracks, gameplay slowdown, missed event triggers). New baseline is native speed; per-game underclock to 75 or 50 for demanding 3D titles like Tony Hawk, Spyro 2/3, Tekken 3.
- v1.20: PERF — `PCSX-ReARMed.opt` set `pcsx_rearmed_gpu_thread_rendering = "async"` (was `sync`). Async threaded GPU is faster on multi-core ARM with negligible visual cost; sync provided no benefit on A15.
- v1.20: Mupen64Plus-Next continues to use Angrylion software RDP + cxd4 RSP — accuracy-over-speed pick documented in v2.39 is intentionally retained.

2026-04-12  Ryan Musante

- v1.19: `Mupen64Plus-Next.opt` — add `mupen64plus-pak1 = "rumble"` (Player 1 Pak: Rumble Pak); valid value verified against upstream `libretro_core_options.h`. Trade-off: removes Controller Pak (memory pak) for player 1 — games that save to controller pak (Mario Kart 64 ghost data, F-Zero X tracks, OoT player notes) can no longer write.
- v1.19: full audit of all 7 `.opt` files against upstream libretro core option headers — every key in every `.opt` file confirmed valid (Mesen 2, Snes9x 1, mGBA 3, Genesis Plus GX 6, Beetle PCE Fast 2, PCSX-ReARMed 4, Mupen64Plus-Next 12 keys post-rumble).
- v1.19: full audit of all 8 per-core `.cfg` files against `libretro/RetroArch/configuration.c` — every frontend override key confirmed valid; no defects found.

2026-04-12  Ryan Musante

- v1.18: README — add missing `Versioning` entry to Table of Contents (added in v1.17 but never linked from TOC).
- v1.18: no changes to `.cfg`, `.opt`, or other README content.

2026-04-12  Ryan Musante

- v1.17: README — add Versioning section above License documenting the `vMAJOR.MINOR` scheme and kernel.org-style CHANGELOG convention.
- v1.17: no changes to `.cfg`, `.opt`, or other README content.

2026-04-12  Ryan Musante

- v1.16: CHANGELOG normalize — strip orphan `retroarch-configs changelog` mid-file header; standardize all entries to kernel.org canonical (date header followed by blank line followed by bullets); unwrap continuation-wrapped bullets; collapse stray multi-blank gaps.
- v1.16: no changes to `.cfg`, `.opt`, or README content; CHANGELOG-only sync.

2026-04-12  Ryan Musante

- v1.15: remove `video_shader` key from all 8 per-core `.cfg` files; CRT shader presets are no longer assigned per core, RetroArch's default applies.
- v1.15: README — drop the `video_shader` row from the cfg key table; rewrite the shader-path note to document the new default behavior and the manual restore path via Quick Menu → Shaders.
- v1.15: no changes to `.opt` core options; no changes to other `.cfg` keys.

2026-04-12  Ryan Musante

- v1.14: sync README archive-count language to the shipped ZIP contents (8 `.cfg` + 7 `.opt` + `config/.gitkeep` = 16 files).
- v1.14: clarify that the README file list covers the functional override files, while the ZIP also contains `config/.gitkeep`.

2026-04-12  Ryan Musante

- v1.13: enable Run-Ahead explicitly in all Tier 1 core override `.cfg` files (`run_ahead_enabled = "true"`, `run_ahead_frames = "2"`) so the shipped Apple TV baseline and companion overrides stay consistent.
- v1.13: sync `README.md`, `CHANGELOG.md`, and Tier 1 `.cfg` key counts with the new per-core Run-Ahead activation model.
- v1.13: document `PCSX-ReARMed.cfg` threaded video as an intentional core-specific fallback and not a blanket recommendation.

2026-04-11  Ryan Musante

- v1.12: sync README, CHANGELOG, and shipped archive contents after the final documentation pass.
- v1.12: keep the ZIP flat under `config/`, and document manual on-device placement under `config/<core_name>/` for RetroArch auto-loading.
- v1.12: remove blank and whitespace-only lines from all shipped `.cfg` and `.opt` files without changing any keys or values.
- v1.12: README now states that shipped filenames and directory examples mirror the archive exactly.

2026-04-11  Ryan Musante

- v1.11: sync README badge and file counts to shipped 8 `.cfg` + 7 `.opt` set

- v1.10: Mupen64Plus-Next.opt set `mupen64plus-rsp-plugin = "cxd4"` for the Angrylion software-rendered profile.
- v1.10: Mupen64Plus-Next.opt set `mupen64plus-ThreadedRenderer = "False"` because that option is for the hardware RDP path, not Angrylion.
- v1.10: README add explicit Angrylion + CXD4 note for Mupen64Plus-Next.

2026-04-11  Ryan Musante

- v1.9.1: sync README, changelog, and shipped config files after layout cleanup.
- v1.9.1: `.cfg` / `.opt` headers no longer carry path, override, or pairing notes; README is now the sole layout reference.
- v1.9.1: README File Structure / File Separation / Installation sections now distinguish ZIP layout (`config/`) from on-device auto-load layout (`config/<core>/`).

2026-04-09  Ryan Musante

- v1.9: Genesis Plus GX.opt — replace genesis_plus_gx_bram (not a real key; silently ignored → per-game Sega CD BRAM isolation never took effect) with the upstream-correct pair genesis_plus_gx_system_bram and genesis_plus_gx_cart_bram (libretro/Genesis-Plus-GX libretro_core_options.h:165,179). CRIT fix.
- v1.9: PCSX-ReARMed.opt — remove pcsx_rearmed_duping_enable (no such core option; frame duping is automatic) and pcsx_rearmed_async_cd (no such core option; CD async is now internal to cdrom-async.h). Both were silently ignored. CRIT fix.
- v1.9: PCSX-ReARMed.opt — remove pcsx_rearmed_frameskip_threshold = "30"; dead config because frameskip_type = "disabled" (threshold only consulted when type = "auto_threshold"). Threshold guidance moved to per-game comment.
- v1.9: mGBA.opt — document that mgba_color_correction is GBA-only (GB/GBC content ignores the setting).
- v1.9: Mupen64Plus-Next.cfg — rewrite header comment so that video_frame_delay_auto = "false" is framed as the mitigation for #14201 rather than implying a standalone bug; rewind #18300 wording tightened.
- v1.9: README Supported Cores table — Genesis Plus GX .opt key count 5 → 6; PCSX-ReARMed .opt key count 7 → 4. Key Verification section expanded with explicit upstream source paths and the list of silently ignored keys removed in this release.

2026-04-06  Ryan Musante

- v1.8.1: PCSX-ReARMed.opt rename pcsx_rearmed_frameskip → pcsx_rearmed_frameskip_type; value "Off" → "disabled" (mainline notaz/pcsx_rearmed renamed the option; legacy key/value silently ignored by current core).

2026-04-06  Ryan Musante

- v1.8: archive ships flat in config/; README documents nested config/<core>/ target layout.
- v1.8: Mesen.opt remove mesen_overclock, mesen_overclock_type; Snes9x.opt remove snes9x_overclock_cycles (now per-game-only, documented in README "Overclocking").
- v1.8: PCSX-ReARMed.opt gpu_thread_rendering "Synchronous" → "sync"; async_cd "enabled" → "async".
- v1.8: Mesen.opt nospritelimit "On" → "enabled"; reduce_dmc_popping "On" → "enabled".
- v1.8: Snes9x.opt overclock_cycles "Light" → "light".
- v1.8: Genesis Plus GX.opt ym2612 "Nuked (YM2612)" → "nuked (ym2612)" (case-sensitive strcmp; capitalised form silently fell back to MAME); audio_filter "Low-Pass" → "low-pass".
- v1.8: mGBA.opt interframe_blending "Smart" → "mix_smart"; audio_low_pass_filter "ON" → "enabled".
- v1.8: README per-game example snes9x_overclock "200" → "200%".
- v1.8: replace "preemptive" with "run-ahead" in 9 locations (README + 7 per-core .cfg comments).
- v1.7: Snes9x.cfg snes9x_overclock documented as per-game-only.
- v1.7: PCSX-ReARMed.opt psxclock safe range (50–57) documented.
- v1.7: Mupen64Plus-Next.cfg, FinalBurn Neo.cfg re-verify upstream refs (libretro/RetroArch#14201, #18300, #16374).
- v1.7: Mupen64Plus-Next.opt rationale for 43screensize = 320x240.
- v1.7: Mesen.opt overclock = Medium / Before NMI rationale (Battletoads/Probotector/Recca).
- v1.7: Genesis Plus GX.opt Nuked YM2612 ~3× CPU cost note.
- v1.7: Tier 2 .cfg per-core audio_latency rationale (64 ms N64, 48 ms PS1).

2026-04-05  Ryan Musante

- v1.6: all 6 Tier 1 cores add run_ahead_frames = 2.
- v1.6: Beetle PCE Fast, mGBA add integer overscale at 4K.
- v1.6: PCSX-ReARMed frameskip Auto → Off, threshold 33 → 30.
- v1.6: Mupen64Plus-Next EnableLODEmulation False → True.
- v1.5: README "(global = false)" added to video_threaded column.
- v1.1–v1.4: per-core CRT shader assignments; audio_resampler_quality = 2 added to Tier 2 cores; Genesis Plus GX integer overscale for 224p; Beetle PCE Fast disable cdimagecache (tvOS RAM constraint); remove redundant keys matching global; fix Mupen64Plus-Next run_ahead_frames 1 → 0; add video_frame_delay_auto = false to PCSX-ReARMed.

2026-04-04  Ryan Musante

- v1.0: initial release — 9 per-core override sets (17 files); Tier 1: 6 cores with runahead from global; Tier 2: 3 cores without JIT, threaded video enabled.
