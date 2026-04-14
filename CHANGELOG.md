2026-04-13  Ryan Musante

- v1.39: SYNC — no `.cfg` / `.opt` changes. CHANGELOG trimmed to the last 5 minor versions; older history removed. README version badge bumped 1.38 → 1.39. Companion `retroarch-appletv4k` bumped to v2.77 (section-header label prefixes stripped from `retroarch.cfg` comments).
- v1.39: `README.md` §1 Supported Cores table — FinalBurn Neo `.opt` keys `1` → `0`. The v1.38 entry that claimed `—` → `1` was off-by-one: `config/FinalBurn Neo.opt` is a comment-only placeholder with no `key = "value"` lines (the dipswitch and cheat keys are per-game only). Notes cell extended to record the placeholder rationale inline.

2026-04-13  Ryan Musante

- v1.38: `config/FinalBurn Neo.opt` — add placeholder; single comment line `# fbneo-dipswitch-* and fbneo-cheat-* are per-game only; no global opt keys required`; reserves the file for future use. `.opt` file count 7 → 8.
- v1.38: `config/Beetle PCE Fast.opt` — add `pce_fast_cdspeed = "2"`; halves TurboGrafx-CD load times; default upstream is `"1"` (1x); per-game override to `"1"` for any title affected. Key verified against `libretro/beetle-pce-fast-libretro/libretro_core_options.h`. `.opt` key count 2 → 3.
- v1.38: `README.md` §1 Supported Cores table — Beetle PCE Fast `.opt` keys 2 → 3; FinalBurn Neo `.opt` keys `—` → 1.
- v1.38: `README.md` §2 File Structure — add `FinalBurn Neo.opt` to flat-config listing; update shipped-file count 7 → 8 `.opt`.
- v1.38: `README.md` §7 Manual Install — add `FinalBurn Neo.opt` to `FinalBurn Neo/` directory tree. Companion `retroarch-appletv4k` bumped to v2.76.

2026-04-13  Ryan Musante

- v1.37: `README.md` §3 File Separation table — 2 Contents cells micro-trimmed. `.cfg` row: "RetroArch frontend settings" → "Frontend settings" ("RetroArch" redundant in a RetroArch config context). `.opt` row: "Core-specific emulation options" → "Core emulation options" ("specific" implied). No semantic changes. Companion `retroarch-appletv4k` bumped to v2.75.

2026-04-13  Ryan Musante

- v1.36: `README.md` §1 Supported Cores table — Mupen64Plus-Next Notes: sentence-separator period removed. "native 320×240. Per-core pins:" → "native 320×240; per-core pins:". Now consistent with all other Notes cells across both tables, which use only semicolons as internal separators. No semantic changes. Companion `retroarch-appletv4k` bumped to v2.74.

2026-04-13  Ryan Musante

- v1.35: `README.md` §1 Supported Cores table — 2 Notes cells brought in line with adjacent rows. FinalBurn Neo: rewind parenthetical tightened from `(conflicts with Run Ahead, [#16374])` to `([#16374] Run Ahead conflict)`, matching the §4 Frontend Override Keys `rewind_enable` Purpose wording adopted in v1.34. Genesis Plus GX: added missing "at 4K" to integer overscale note — 5 of 6 other Tier 1 rows already carried the qualifier; Genesis was the lone exception. No semantic changes. Companion `retroarch-appletv4k` bumped to v2.72.

2026-04-13  Ryan Musante

- v1.34: `README.md` §4 Frontend Override Keys table — Notes column condensed (5 rows). Removed verbose preamble clauses while preserving all key names, issue numbers, and per-core distinctions: `run_ahead_frames` dropped "under single-instance run-ahead"; `video_threaded` collapsed source citation to short form; `audio_latency` tightened companion-version reference; `video_frame_delay_auto` removed redundant "matches global" clause body; `rewind_enable` restructured as issue-reference list. No semantic changes. Companion `retroarch-appletv4k` bumped to v2.71.
