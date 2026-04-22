2026-04-22  Ryan Musante

- v3.7: 4 documentation-only factual corrections; 0 .cfg / .opt
  key value changes. Totals unchanged: cfg 55, opt 27.

  README:
  - §1 Mupen row: "A15 is 2P+4E; fallbacks" → "A15 is 2P+3E
    (binned, 1 E-core fused off); fallbacks". Apple TV 4K 3rd
    Gen ships a binned A15 with one efficiency core fused off,
    a distinct variant from the iPhone 13/14 A15 despite
    sharing the marketing name.
  - §4 `audio_latency` row: "scheduler variance on 2P+4E A15"
    → "scheduler variance on 2P+3E A15".

  `Mupen64Plus-Next.cfg` (9 keys; all values unchanged):
  - Line 15 inline comment: "(2P+4E A15)" → "(2P+3E binned A15)".

  `Mupen64Plus-Next.opt` (6 keys; all values unchanged):
  - Line 16 inline comment: "(2P+4E)" → "(2P+3E binned)".

  `CHANGELOG.md`:
  - Prior v3.3 / v3.5 / v3.6 entries corrected in-place at
    4 locations where they stated 2P+4E. The v3.3 `.opt` note
    also corrects the derivative claim that "all threads"
    dispatches 6 RDP workers — on 2P+3E it dispatches 5.

  All 8 `.cfg` / `.opt` key=value lines byte-identical to v3.6.

  Version badge 3.6 → 3.7. Companion bumped to v3.7 (README
  §6/§9/§10 doc-only edits for 2P+4E → 2P+3E correction plus
  #16685 Known Issue addition).

  Sources: https://en.wikipedia.org/wiki/Apple_A15;
  https://www.macrumors.com/2022/11/14/new-apple-tv-5-core-cpu/

2026-04-21  Ryan Musante

- v3.6: 1 .cfg value change + 1 .cfg comment doc-sync.
  Companion v3.6 ships 2 global cfg value changes; this sync
  re-anchors Tier 2 audio latencies to the new global 48.

  `Mupen64Plus-Next.cfg` (9 keys, unchanged count):
  - `audio_latency` "96" → "64" (HIGH; paired with companion
    `retroarch.cfg` v3.6 global change "32" → "48". New Tier 2
    pin is +16 ms over global for E-core + GC + scheduler
    variance on 2P+3E A15; v3.5 +64 margin was
    overprovisioned against the aggressive global 32).
  - Inline comment updated to reflect new absolute value and
    +16 ms rationale.

  `PCSX-ReARMed.cfg` (9 keys; value unchanged):
  - `audio_latency = "48"` retained. Inline comment rewritten:
    rationale is now "drift-guard mirror of v3.6 global 48"
    instead of "tighter than global 64" (pre-v3.5). Pin
    prevents silent drift if the global is later raised.

  All 8 `.cfg` header "paired with retroarch-appletv4k v3.X"
  stamps bumped v3.5 → v3.6.

  Totals unchanged: cfg 55, opt 27.
  .opt content byte-identical to v3.5.

  README:
  - §1 Mupen row: `audio_latency = "96"` → `"64"` (with v3.6
    change note).
  - §4 `audio_latency` row: values "48, 96" → "48, 64";
    purpose split rewritten for new global context
    (PCSX drift-guard mirror; Mupen +16 ms variance margin).
  - Badge bumped 3.5 → 3.6.

  Version badge 3.5 → 3.6. Companion bumped to v3.6
  (`retroarch.cfg` `fastforward_ratio` "5.0" → "4.0" +
  `audio_latency` "32" → "48"; 90 keys otherwise byte-identical).

2026-04-21  Ryan Musante

- v3.5: SYNC — companion ships global cfg latency + hardening refresh.
  - All 8 `.cfg` header "paired with retroarch-appletv4k v3.X"
    stamps bumped v3.4 → v3.5.
  - README badge bumped 3.4 → 3.5.
  - Totals unchanged: cfg 55, opt 27.
  - .cfg / .opt content byte-identical to v3.4 (apart from the
    single paired-with stamp line).
  - Version badge 3.4 → 3.5. Companion bumped to v3.5
    (`retroarch.cfg` 65 → 90 keys: 3 modifications
    [`menu_pause_libretro` "true" → "false",
    `pause_nonactive` "true" → "false",
    `audio_latency` "64" → "32"] and 25 additions across
    Menu/UI (6), Video (8), Input (2), Latency (1),
    Security (2), and a new Null drivers section (6);
    no impact to per-core overrides — `audio_latency`,
    `menu_pause_libretro`, and `pause_nonactive` are not
    pinned per-core, so Tier 1 cores inherit the new global
    defaults while Tier 2 `audio_latency` pins PCSX=48,
    Mupen=96 continue to override up).

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
  + tunes Angrylion multithread for A15's 2P+3E cluster.

  `Mupen64Plus-Next.cfg` (5 → 9 keys):
  - `audio_latency = "96"` (HIGH; +32 ms over global 64 for E-core
    stragglers + GC + scheduler variance on 2P+3E A15).
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
    (HIGH; A15 2P+3E; "all threads" dispatches 5 RDP workers,
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
