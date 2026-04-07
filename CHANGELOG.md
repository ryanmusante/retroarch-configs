retroarch-configs changelog

2026-04-06  Ryan Musante

- v1.8: archive ships flat in config/; README documents nested
  config/<core>/<core>.cfg target layout.
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
