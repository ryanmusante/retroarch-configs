retroarch-configs changelog


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
