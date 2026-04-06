retroarch-configs changelog


2026-04-05  Ryan Musante

- Tagged as v1.6

- All 6 Tier 1 cores: add run_ahead_frames = 2 for
    2-frame preemptive latency reduction (Snes9x, Mesen,
    mGBA, Genesis Plus GX, Beetle PCE Fast, FinalBurn Neo).
- Beetle PCE Fast, mGBA: add video_scale_integer_scaling = 1
    for overscale at 4K, matching Mesen/Snes9x/Genesis Plus GX.
- PCSX-ReARMed: frameskip Auto → Off, threshold 33 → 30;
    A15 runs most PS1 at full speed without JIT.
- Mupen64Plus-Next: EnableLODEmulation False → True; LOD
    is GPU-side, zero cost at 320×240 on A15.
- melonDS DS: add screen_gap = 0.
- README.md: update .cfg/.opt key counts; expand Frontend
    Override Keys table with Tier 1 run_ahead_frames = 2 and
    overscale for PCE/GBA; update tier definition.

- Tagged as v1.5

- README.md: add "(global = false)" to video_threaded Purpose
    column for consistency with audio_resampler_quality pattern.

- Tagged as v1.1 – v1.4

- Add per-core CRT shader assignments: crt-easymode (Tier 1),
    zfast_crt (Tier 2 N64/PS1), none (melonDS DS).
- Add audio_resampler_quality = 2 to all Tier 2 cores.
- Genesis Plus GX: add integer overscale for 224p at 4K.
- Beetle PCE Fast: disable cdimagecache (tvOS RAM constraint).
- Remove redundant keys matching global: rewind_enable (9 files),
    run_ahead_frames = 1 (6 Tier 1 files).
- Fix Mupen64Plus-Next run_ahead_frames: 1 → 0.
- Add video_frame_delay_auto = false to PCSX-ReARMed.
- README.md: add CRT Shaders section; update key counts.

2026-04-04  Ryan Musante

- Tagged as v1.0

- Initial release: 9 per-core override sets for Apple TV 4K.
- Tier 1: Beetle PCE Fast, FinalBurn Neo, Genesis Plus GX,
    Mesen, mGBA, Snes9x — runahead inherited from global.
- Tier 2: Mupen64Plus-Next, PCSX-ReARMed, melonDS DS — no JIT;
    runahead/preemptive frames disabled; threaded video enabled.
- 17 files: 9 .cfg (frontend overrides) + 8 .opt (core options).
- Core option key prefix verified: melonds_ (not melondsds_).
