2026-04-20  Ryan Musante

- v3.4: README drift fix; .cfg/.opt byte-identical to v3.3.
  - README §4 `run_ahead_frames` row: "All 7 Tier 1 cores" →
    "All 6 Tier 1 cores" (MED; stale count from pre-v3.3 when
    row value column also read "1, 2"; §1 ships 6 Tier 1 cores
    and 6 `.cfg` files set `run_ahead_frames = "2"`).
  - All 8 `.cfg` header "paired with retroarch-appletv4k v3.X"
    stamps bumped v3.3 → v3.4.
  - README badge bumped 3.3 → 3.4.
  - Totals unchanged: cfg 55, opt 27.
  - Version badge 3.3 → 3.4. Companion bumped to v3.4 (SYNC;
    `retroarch.cfg` header stamps only; 65 keys byte-identical).

2026-04-19  Ryan Musante

- v3.3: 2 .cfg files modified (+6 keys), 1 .opt value change.
  Restores Mupen v1.56 stutter pins + adds Tier 2 parity on PCSX
  + tunes Angrylion multithread for A15's 2P+4E cluster.

  `Mupen64Plus-Next.cfg` (5 → 9 keys):
  - `audio_latency = "96"` (HIGH; +32 ms over global 64 for E-core
    stragglers + GC + scheduler variance on 2P+4E A15).
  - `video_max_swapchain_images = "3"` (HIGH; absorbs frame-time
    variance on CPU-bound Tier 2 core; Metal triple-buffer no
    pacing penalty).
  - `audio_sync = "false"` (HIGH; video-driven pacing drops frames
    cleanly vs pitch rubber-band under global true).
  - `autosave_interval = "0"` (HIGH; prevents ~16–40 ms purgeable-
    cache stall from SRAM write every 2.5 min on 64 GB model;
    save-on-close retained via global `savestate_auto_save`).
  - Header rewritten with platform-forced plugin rationale and
    pin references (#14978, #14201, #18300).

  `Mupen64Plus-Next.opt` (1 value):
  - `mupen64plus-angrylion-multithread` "all threads" → "2"
    (HIGH; A15 2P+4E; "all threads" dispatches 6 RDP workers,
    E-core tail latency drives audio stutter; "2" pins to P-cores;
    fallbacks "3", "4"; do not revert to "all threads" on
    heterogeneous ARM).
  - Header rewritten with platform-forced plugin stack rationale.

  `PCSX-ReARMed.cfg` (7 → 9 keys; Tier 2 parity):
  - `video_max_swapchain_images = "3"` (MED; same rationale).
  - `autosave_interval = "0"` (MED; prevents memcard-write stall).
  - `audio_sync` NOT pinned per-core — core-level
    `pcsx_rearmed_frameskip_type = "auto_threshold"` handles
    buffer pressure orthogonally; retain global true.

  Totals: cfg 49 → 55, opt unchanged at 27.

  README updates:
  - §1 Mupen row: .cfg 5 → 9; notes rewritten for v3.3 pins +
    platform-forced stack.
  - §1 PCSX row: .cfg 7 → 9; notes appended with v3.3 pins.
  - §4 `audio_latency` row: values "48" → "48, 96"; purpose split.
  - §4 new rows: `video_max_swapchain_images`, `audio_sync`,
    `autosave_interval`.
  - §4 `run_ahead_frames` row: values "1, 2" → "2" (PCE Fast
    parity correction).
  - Version badge 3.2 → 3.3. Companion bumped to v3.3
    (`fastforward_ratio` 0.0 → 5.0; cfg 65 keys unchanged).

2026-04-19  Ryan Musante

- v3.2: 2 .opt enum-mismatch correctness fixes.
  - `mGBA.opt`: `mgba_color_correction` "Game Boy Advance" → "Auto"
    (HIGH; prior value was the display label, not the internal
    enum OFF/GBA/GBC/Auto. RetroArch matched on internal value
    via `string_is_equal`, so the label silently failed match and
    fell back to OFF — color correction has been effectively
    disabled since introduction. Auto chosen over GBA because
    the core handles GB/GBC/GBA per §1 — Auto applies per-system
    correction. Real behavior change.).
  - `mGBA.opt` header updated.
  - `Genesis Plus GX.opt`: `genesis_plus_gx_audio_filter` "off" →
    "disabled" (MED; "off" was outside enum disabled/low-pass/EQ
    and silently fell back to default disabled — same effect.
    Correctness only.).
  - opt unchanged at 27, cfg unchanged at 49.
  - README §1 Genesis + mGBA rows: v3.2 markers appended.
  - Version badge 3.1 → 3.2. Companion bumped to v3.2 (no
    `retroarch.cfg` value changes; 2 stale README rows removed).

2026-04-19  Ryan Musante

- v3.1: 5 .opt files modified (net +0 keys).
  - `Genesis Plus GX.opt`: `genesis_plus_gx_audio_filter`
    "low-pass" → "off" (CPU headroom);
    `genesis_plus_gx_lowpass_range` "55" key removed (inert).
    Keys 7 → 6.
  - `mGBA.opt`: `mgba_interframe_blending` "mix_smart" →
    "disabled" (per-frame GPU blend cost removed; flicker on
    Zelda MC, F-Zero GP Legend, MK SC);
    `mgba_audio_low_pass_filter` "enabled" → "disabled"
    (IIR cost removed). Keys unchanged at 3.
  - `PCSX-ReARMed.opt`: `pcsx_rearmed_frameskip_type` "disabled"
    → "auto_threshold"; new key
    `pcsx_rearmed_frameskip_threshold = "33"` (33% audio-buffer;
    guarantees real-time on Spyro 2/3, THPS, Tekken 3 at cost
    of visible frame drops). Keys 5 → 6.
  - `Beetle PCE Fast.opt`: `pce_fast_cdspeed` "2" → "4" (may
    desync Ys IV, Dracula X — revert per-game). Keys at 3.
  - `Mesen.opt`: `mesen_reduce_dmc_popping` "enabled" → "disabled"
    (sub-1% CPU; pops on Skate or Die 2, Ninja Gaiden III).
    Keys at 2.
  - All .opt headers updated. opt unchanged at 27 (Genesis -1,
    PCSX +1); cfg unchanged at 49.
  - README §1: Beetle, Genesis, Mesen, mGBA, PCSX rows updated
    with v3.1 markers.
  - README §5 Shaders: rewritten for per-core Save Core Preset
    workflow.
  - Version badge 3.0 → 3.1. Companion bumped to v3.1
    (3 `retroarch.cfg` value changes).

2026-04-19  Ryan Musante

- v3.0: MAJOR — version-sync release.
  - Establishes lockstep versioning: both `retroarch-configs` and
    `retroarch-appletv4k` share a single MAJOR.MINOR tag from
    here forward.
  - Pre-sync: configs v1.57, appletv4k v2.95.
  - No .cfg/.opt content changes — 16 files byte-identical to
    v1.57 shipping state.
  - README §11 Versioning: rewritten to document lockstep
    convention; matches companion §13 byte-for-byte.
  - Totals unchanged: cfg 49, opt 27.
  - Version badge 1.57 → 3.0. Companion bumped to v3.0.
