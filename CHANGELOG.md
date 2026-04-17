2026-04-16  Ryan Musante

- v1.43: All 16 per-core `.cfg`/`.opt` files — strip all `#` comment and blank lines; total 112L → 65L. Keys, values, and key counts unchanged (54 keys across 16 files; per-core totals match v1.42 §1 table exactly: Beetle 5/3, FBN 6/0, Genesis 5/7, Mesen 5/2, mGBA 5/4, Snes9x 5/1, Mupen 4/8, PCSX 5/5). `README.md` §11 Versioning: CHANGELOG style description `kernel.org date<TAB>name` → `GNU ChangeLog style (YYYY-MM-DD<SP><SP>Author Name)` — actual file separator is double space. Version badge 1.42 → 1.43. Companion `retroarch-appletv4k` bumped to v2.81.

2026-04-15  Ryan Musante

- v1.42: Apple TV 4K optimization pass; 4 explicit `.opt` pins added for drift-guard consistency (all keys match prior upstream defaults — no behavior change). `config/Mupen64Plus-Next.opt` — add `mupen64plus-angrylion-multithread = "all threads"` (Angrylion uses all 6 A15 cores by default; explicit guards future thermal tuning). `config/PCSX-ReARMed.opt` — add `pcsx_rearmed_neon_enhancement_enable = "disabled"` (expensive upscale; no benefit on no-JIT A15; guards accidental flip). `config/mGBA.opt` — add `mgba_audio_low_pass_range = "60"` (pairs with `audio_low_pass_filter` already set). `config/Genesis Plus GX.opt` — add `genesis_plus_gx_render = "single field"` (lighter than double field on A15). `.opt` key counts: Genesis Plus GX 6 → 7, mGBA 3 → 4, Mupen64Plus-Next 7 → 8, PCSX-ReARMed 4 → 5.
- v1.42: `README.md` §1 Supported Cores table — `.opt` key counts updated for the 4 affected cores. Version badge 1.41 → 1.42. Companion `retroarch-appletv4k` bumped to v2.80 (SYNC bump; no `retroarch.cfg` changes).

2026-04-15  Ryan Musante

- v1.41: All 8 per-core `.cfg` files now set explicit `video_shader` paths using `../shaders/shaders_slang/` relative references; shaders no longer inherited from global `retroarch.cfg`. Tier 1 CRT cores: `crt-easymode.slangp`; mGBA: `handheld/lcd-grid-v2.slangp` (GBA/GB/GBC were handheld LCDs, not CRTs); Tier 2: `zfast_crt.slangp` (path format updated from `shaders_slang/crt/`). `.cfg` key counts: Beetle PCE Fast 4 → 5, FinalBurn Neo 5 → 6, Genesis Plus GX 4 → 5, Mesen 4 → 5, Snes9x 4 → 5, mGBA 4 → 5, PCSX-ReARMed 3 → 5; Mupen64Plus-Next unchanged at 4.
- v1.41: `config/PCSX-ReARMed.cfg` — add `run_ahead_enabled = "false"` (explicit interpreter safety guard; previously inherited from global) and `video_scale_integer_scaling = "1"` (integer overscale; PS1 variable width 256–640 may shift borders on mode switch).
- v1.41: `config/Mupen64Plus-Next.cfg`, `config/PCSX-ReARMed.cfg` — stale comments referencing "override global easymode" updated to "zfast_crt Tier 2" (global `video_shader` removed in companion v2.79).
- v1.41: `README.md` — §1 Supported Cores table: CRT Shader column renamed to Shader; `(global)` / `(override)` qualifiers removed; mGBA shader updated to lcd-grid-v2; PCSX-ReARMed Notes extended for `run_ahead_enabled` + integer overscale; `.cfg` key counts updated. §4 Frontend Override Keys table: `video_shader` row expanded to all 8 cores with 3 shader values; `run_ahead_enabled` row updated for PCSX explicit `false`; `video_scale_integer_scaling` row extended to include PCSX. §5 CRT Shaders rewritten (no global default; per-core explicit). Intro paragraph updated for explicit pin rationale. Version badge 1.40 → 1.41. Companion `retroarch-appletv4k` bumped to v2.79.

2026-04-13  Ryan Musante

- v1.40: `README.md` §2 File Structure — reorder the flat-config tree so `mGBA.cfg` / `mGBA.opt` appear at the end (after `Snes9x.opt`) instead of between `Mesen` and `Mupen64Plus-Next`. §7 Manual Install tree and actual filesystem `ls` output both place `mGBA` last (ASCII sort: uppercase sorts before lowercase). §2 was the lone divergence — now both trees and the shipped ZIP layout agree. §1 Supported Cores table left unchanged (Tier-grouped, not filesystem-ordered). No content or key changes. Companion `retroarch-appletv4k` stays at v2.78.

2026-04-13  Ryan Musante

- v1.39: SYNC — no `.cfg` / `.opt` changes. CHANGELOG trimmed to the last 5 minor versions; older history removed. README version badge bumped 1.38 → 1.39. Companion `retroarch-appletv4k` bumped to v2.77 (section-header label prefixes stripped from `retroarch.cfg` comments).
- v1.39: `README.md` §1 Supported Cores table — FinalBurn Neo `.opt` keys `1` → `0`. The v1.38 entry that claimed `—` → `1` was off-by-one: `config/FinalBurn Neo.opt` is a comment-only placeholder with no `key = "value"` lines (the dipswitch and cheat keys are per-game only). Notes cell extended to record the placeholder rationale inline.
