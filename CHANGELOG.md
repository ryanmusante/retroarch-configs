retroarch-configs changelog

2026-04-11  Ryan Musante

- v1.9.1: sync README, changelog, and shipped config files after layout cleanup.
- v1.9.1: `.cfg` / `.opt` headers no longer carry path, override, or pairing notes; README is now the sole layout reference.
- v1.9.1: README File Structure / File Separation / Installation sections now distinguish ZIP layout (`config/`) from on-device auto-load layout (`Config/<core>/`).

2026-04-09  Ryan Musante

- v1.9: Genesis Plus GX.opt — replace genesis_plus_gx_bram (not a real key;
  silently ignored → per-game Sega CD BRAM isolation never took effect)
  with the upstream-correct pair genesis_plus_gx_system_bram and
  genesis_plus_gx_cart_bram (libretro/Genesis-Plus-GX
  libretro_core_options.h:165,179). CRIT fix.
- v1.9: PCSX-ReARMed.opt — remove pcsx_rearmed_duping_enable
  (no such core option; frame duping is automatic) and pcsx_rearmed_async_cd
  (no such core option; CD async is now internal to cdrom-async.h).
  Both were silently ignored. CRIT fix.
- v1.9: PCSX-ReARMed.opt — remove pcsx_rearmed_frameskip_threshold = "30";
  dead config because frameskip_type = "disabled" (threshold only consulted
  when type = "auto_threshold"). Threshold guidance moved to per-game comment.
- v1.9: mGBA.opt — document that mgba_color_correction is GBA-only
  (GB/GBC content ignores the setting).
- v1.9: Mupen64Plus-Next.cfg — rewrite header comment so that
  video_frame_delay_auto = "false" is framed as the mitigation for #14201
  rather than implying a standalone bug; rewind #18300 wording tightened.
- v1.9: README Supported Cores table — Genesis Plus GX .opt key count
  5 → 6; PCSX-ReARMed .opt key count 7 → 4. Key Verification section
  expanded with explicit upstream source paths and the list of silently
  ignored keys removed in this release.

2026-04-06  Ryan Musante

- v1.8.1: PCSX-ReARMed.opt rename pcsx_rearmed_frameskip → pcsx_rearmed_frameskip_type;
  value "Off" → "disabled" (mainline notaz/pcsx_rearmed renamed the option;
  legacy key/value silently ignored by current core).

2026-04-06  Ryan Musante

- v1.8: archive ships flat in config/; README documents nested
  Config/<core>/ target layout.
- v1.8: Mesen.opt remove mesen_overclock, mesen_overclock_type;
  Snes9x.opt remove snes9x_overclock_cycles (now per-game-only,
  documented in README "Overclocking").
- v1.8: PCSX-ReARMed.opt gpu_thread_rendering "Synchronous" → "sync";
  async_cd "enabled" → "async".
- v1.8: Mesen.opt nospritelimit "On" → "enabled";
  reduce_dmc_popping "On" → "enabled".
- v1.8: Snes9x.opt overclock_cycles "Light" → "light".
- v1.8: Genesis Plus GX.opt ym2612 "Nuked (YM2612)" → "nuked (ym2612)"
  (case-sensitive strcmp; capitalised form silently fell back to MAME);
  audio_filter "Low-Pass" → "low-pass".
- v1.8: mGBA.opt interframe_blending "Smart" → "mix_smart";
  audio_low_pass_filter "ON" → "enabled".
- v1.8: README per-game example snes9x_overclock "200" → "200%".
- v1.8: replace "preemptive" with "run-ahead" in 9 locations
  (README + 7 per-core .cfg comments).
- v1.7: melonDS DS Tier 2 → Tier 3 (Compromised) — no touchscreen on tvOS.
- v1.7: Snes9x.cfg snes9x_overclock documented as per-game-only.
- v1.7: PCSX-ReARMed.opt psxclock safe range (50–57) documented.
- v1.7: Mupen64Plus-Next.cfg, FinalBurn Neo.cfg re-verify upstream refs
  (libretro/RetroArch#14201, #18300, #16374).
- v1.7: Mupen64Plus-Next.opt rationale for 43screensize = 320x240.
- v1.7: melonDS DS.opt jit_enable cross-reference to .cfg.
- v1.7: Mesen.opt overclock = Medium / Before NMI rationale
  (Battletoads/Probotector/Recca).
- v1.7: Genesis Plus GX.opt Nuked YM2612 ~3× CPU cost note.
- v1.7: Tier 2/3 .cfg per-core audio_latency rationale
  (64 ms N64, 48 ms PS1/DS).

2026-04-05  Ryan Musante

- v1.6: all 6 Tier 1 cores add run_ahead_frames = 2.
- v1.6: Beetle PCE Fast, mGBA add integer overscale at 4K.
- v1.6: PCSX-ReARMed frameskip Auto → Off, threshold 33 → 30.
- v1.6: Mupen64Plus-Next EnableLODEmulation False → True.
- v1.6: melonDS DS add screen_gap = 0.
- v1.5: README "(global = false)" added to video_threaded column.
- v1.1–v1.4: per-core CRT shader assignments;
  audio_resampler_quality = 2 added to Tier 2 cores;
  Genesis Plus GX integer overscale for 224p;
  Beetle PCE Fast disable cdimagecache (tvOS RAM constraint);
  remove redundant keys matching global;
  fix Mupen64Plus-Next run_ahead_frames 1 → 0;
  add video_frame_delay_auto = false to PCSX-ReARMed.

2026-04-04  Ryan Musante

- v1.0: initial release — 9 per-core override sets (17 files);
  Tier 1: 6 cores with runahead from global;
  Tier 2: 3 cores without JIT, threaded video enabled.
