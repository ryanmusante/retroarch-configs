retroarch-configs changelog

2026-04-06  Ryan Musante

- Tagged as v1.8
- Archive ships files flat in config/ for manual placement; README documents
  the required nested config/<core>/<core>.cfg target layout per RetroArch's
  override hierarchy.
- Mesen.opt: removed mesen_overclock and mesen_overclock_type. Snes9x.opt:
  removed snes9x_overclock_cycles. Both are now per-game-only and documented
  in README "Overclocking" section. Avoids breaking timing-sensitive titles
  by default.
- README: added "Overclocking" section with per-game examples for Mesen CPU,
  Snes9x CPU, and Snes9x SuperFX. Updated key counts (Mesen opt 4->2,
  Snes9x opt 2->1) and table of contents.
- PCSX-ReARMed.opt: gpu_thread_rendering "Synchronous" -> "sync";
  async_cd "enabled" -> "async" (upstream accepts disabled|sync|async and
  sync|async|precache respectively).
- Mesen.opt: nospritelimit "On" -> "enabled"; overclock_type "Before NMI"
  -> "Before NMI (Recommended)"; reduce_dmc_popping "On" -> "enabled".
- Snes9x.opt: overclock_cycles "Light" -> "light" (lowercase per upstream).
- Genesis Plus GX.opt: ym2612 "Nuked (YM2612)" -> "nuked (ym2612)" (upstream
  strcmp is case-sensitive; capitalised form silently fell back to MAME);
  audio_filter "Low-Pass" -> "low-pass".
- mGBA.opt: interframe_blending "Smart" -> "mix_smart" (internal value, not
  the menu label); audio_low_pass_filter "ON" -> "enabled" (upstream strcmp
  against "enabled").
- Snes9x.cfg: synced per-game comment example to snes9x_overclock = "200%".
- README per-game example: snes9x_overclock "200" -> "200%" (upstream key
  takes percent strings 50%-500%).
- Terminology: replaced "preemptive" with "run-ahead" in 9 locations
  (README + 7 per-core .cfg comments). RetroArch run_ahead_frames and
  preemptive_frames_enable are distinct features.

2026-04-06  Ryan Musante

- Tagged as v1.7
- melonDS DS: demoted from Tier 2 to Tier 3 (Compromised) — no touchscreen
  on tvOS makes touch-required titles unplayable; software 1× already
  forced via video_shader_enable = false.
- Snes9x.cfg: clarified snes9x_overclock as per-game-only key (never set
  globally); README per-game section gained Star Fox example.
- PCSX-ReARMed.opt: documented psxclock safe range (50–57) and instability
  risk above default; added drc → .cfg cross-reference.
- Mupen64Plus-Next.cfg, FinalBurn Neo.cfg: re-verified upstream issue refs
  (libretro/RetroArch#14201, #18300, #16374) on 2026-04-06.
- Mupen64Plus-Next.opt: added rationale for 43screensize = 320x240.
- melonDS DS.opt: added jit_enable → .cfg cross-reference comment.
- Mesen.opt: added rationale for overclock = Medium / Before NMI
  (Battletoads/Probotector/Recca slowdown reduction).
- Genesis Plus GX.opt: added cost note for Nuked YM2612 (~3× CPU).
- All Tier 2/3 .cfg: documented per-core audio_latency rationale
  (64 ms for N64, 48 ms for PS1/DS).
- README: added Tier 3 definition; updated audio_latency and
  run_ahead_frames key descriptions; added per-game SuperFX example;
  added upstream issue verification line.

2026-04-05  Ryan Musante

- Tagged as v1.6
- All 6 Tier 1 cores: add run_ahead_frames = 2.
- Beetle PCE Fast, mGBA: add integer overscale at 4K.
- PCSX-ReARMed: frameskip Auto→Off, threshold 33→30.
- Mupen64Plus-Next: EnableLODEmulation False→True.
- melonDS DS: add screen_gap = 0.
- README: update key counts and tier definitions.
- Tagged as v1.5
- README: add "(global = false)" to video_threaded column.
- Tagged as v1.1 – v1.4
- Add per-core CRT shader assignments.
- Add audio_resampler_quality = 2 to Tier 2 cores.
- Genesis Plus GX: add integer overscale for 224p at 4K.
- Beetle PCE Fast: disable cdimagecache (tvOS RAM constraint).
- Remove redundant keys matching global (rewind_enable, run_ahead_frames).
- Fix Mupen64Plus-Next run_ahead_frames: 1→0.
- Add video_frame_delay_auto = false to PCSX-ReARMed.

2026-04-04  Ryan Musante

- Tagged as v1.0
- Initial release: 9 per-core override sets (17 files).
- Tier 1: 6 cores with runahead from global.
- Tier 2: 3 cores without JIT, threaded video enabled.
