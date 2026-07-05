# 4.1 - 2026-07-05

- v4.1: Beetle PCE Fast.opt drops `pce_fast_cdimagecache = "enabled"` — reverts to upstream default `"disabled"` (libretro/beetle-pce-fast-libretro `libretro_core_options.h`, default `"disabled"`). Full CD-image RAM precache is imprudent as a global default on the 4 GB shared-RAM, fanless Apple TV 4K 3rd Gen (binned A15, 2P+3E, 4 GB LPDDR verified Apple/EveryMac/FlatpanelsHD): a multi-hundred-MB precache footprint contends with tvOS + RetroArch against a fixed 4 GB and risks background jettison / memory-pressure stalls. Seek-latency relief retained as a per-game opt-in (`pce_fast_cdimagecache="enabled"`), consistent with the repo's per-game-override policy for hardware-dependent keys (cf. Overclocking; cdspeed `"4"`). opt 3 keys -> 2 keys.
- Beetle PCE Fast.opt: header rewritten to reflect 2 keys and the per-game precache guidance; `cdspeed = "2"` and `nospritelimit = "enabled"` unchanged (real flips from defaults `"1"` / `"disabled"`).
- README.md: Supported Cores — Beetle PCE Fast `.opt` count 3 -> 2; Notes drop "CD precache", add per-game precache / 4 GB rationale.
- README.md: version badge 4.0 -> 4.1; paired badge `retroarch--appletv4k v4.0` -> `v4.1`.
- config/*.cfg: 7 files; bump header version stamps and "paired with retroarch-appletv4k" stamps v4.0 -> v4.1. Bodies byte-identical to v4.0 (verified line-2-onward diff = 0 for all 7).
- Companion v4.1: retroarch.cfg `audio_latency "48" -> "64"` (clears the libretro optimal-vsync 3-frame safe floor; matches RetroArch upstream `config.def.h` DEFAULT_OUT_LATENCY 64 on the non-Android branch); 74 keys otherwise unchanged; header + paired stamp v4.0 -> v4.1. See retroarch-appletv4k v4.1 entry.
- CHANGELOG.md: trim v3.25 entry per 5-release retention; retained entries are now v3.26-v3.28 + v4.0 + v4.1.
- cfg 21, opt 19 -> 18, cfg+opt 40 -> 39.

# 4.0 - 2026-05-16

- v4.0: MAJOR bump — README restructured to ry-install style (breaking anchor schema); 7 `.cfg` paired stamps v3.28 -> v4.0; 7 `.opt` byte-identical (frontend-version-independent per v3.12 design).
- README.md: full restructure to ry-install template. Numbered sections dropped (`## 1. Supported Cores ... ## 12. License` -> unnumbered `## Supported Cores ... ## License`); all `§N` cross-references retired. Table of Contents renamed `## Table of Contents` -> `## Contents`; format switched from numbered list to bulleted list.
- README.md: **BREAKING** anchor schema change — slugs drop the leading `N-` prefix (e.g. `#1-supported-cores` -> `#supported-cores`, `#4-frontend-override-keys` -> `#frontend-override-keys`, `#7-manual-install-per-core-override-path` -> `#layout`). External inbound links to old anchors will 404. Motivates MAJOR bump per versioning policy.
- README.md: section consolidation — §2 File Structure + §6 Installation + §7 Manual Install: Per-Core Override Path collapsed into a single `## Layout` section; §3 File Separation absorbed into `## Configuration` as the lead table. Net section count 12 -> 9.
- README.md: 3 reference subsections folded into `<details>` collapsibles — `Zip contents (flat)` (parented under Layout), `Frontend override keys` (under Configuration), `Shaders` (under Configuration). Default-collapsed; rendered surface area roughly halved.
- README.md: GitHub admonitions replace prose-style warnings — Quick Start gains `> [!IMPORTANT]` (per-core-path miss = silent Tier 1 run-ahead disable, was a buried Verify-callout line in §7); Configuration gains `> [!WARNING]` (mixing `.cfg`/`.opt` = silent failure, was an unmarked paragraph under §3).
- README.md: header gains `[![paired]](...)` cross-link badge to companion repo (third badge slot, between cores count and license).
- README.md: License section adopts `MIT © 2026 Ryan Musante` attribution form (matches ry-install convention; supersedes the bare `[MIT](LICENSE)` link).
- README.md: badge 3.28 -> 4.0; paired badge `retroarch--appletv4k v3.28` -> `v4.0`.
- README.md: byte size 7792 -> 6914 (-11.3%); line count 142 -> 173 (+31 from `<details>` / admonition markup; body content tighter).
- config/*.cfg: 7 files; bump header version stamps and "paired with retroarch-appletv4k" stamps v3.28 -> v4.0. Bodies byte-identical to v3.28 (verified line-2-onward diff = 0 for all 7).
- config/*.opt: 7 files; unchanged. No version stamps (frontend-version-independent per v3.12 design — same rule as v3.27 entry); v3.28 changelog's reference to ".opt paired stamps" was loose wording — actual .opt headers carry no version field.
- CHANGELOG.md: trim v3.24 entry per 5-release retention; retained entries are now v3.25-v3.28 + v4.0.
- cfg 21, opt 19, cfg+opt 40 — unchanged.

# 3.28 - 2026-05-10

- v3.28: FinalBurn Neo.cfg drops `run_ahead_secondary_instance` pin; Beetle PCE Fast.opt rebases `pce_fast_cdspeed`; 7 `.cfg` + 7 `.opt` paired stamps to v3.28.
- FinalBurn Neo.cfg: drop `run_ahead_secondary_instance = "true"` line. Per upstream FBN libretro README at libretro/FBNeo `src/burner/libretro/README.md`: "This core widely supports the RetroArch input latency reduction features, with runahead single instance and preemptive frames being the recommended methods. Proper support for runahead second instance is not guaranteed because it doesn't exist in standalone FBNeo unlike the other methods." Prior pin's "per FBN core maintainer" attribution conflicted with the upstream-published guidance. FBN now inherits global `run_ahead_secondary_instance = "false"` for single-instance runahead behavior matching the upstream-recommended method. cfg 4 keys -> 3 keys.
- Beetle PCE Fast.opt: `pce_fast_cdspeed "4" -> "2"`. Per upstream beetle-pce-fast-libretro advisory at `libretro_core_options.h`: "Higher values enable faster loading times but can cause issues with a couple of games"; aligns with community-standard reference at wiki.rg35xx.com. Per-game override to "4" remains available where compatibility permits and faster CD streaming is preferred. Header inline comment updated to reflect "2x" streaming + "per-game override available where compatibility permits"; opt 3 keys unchanged.
- All 7 `.cfg` paired stamps v3.27 -> v3.28; 6 `.cfg` bodies byte-identical to v3.27 (only FinalBurn Neo.cfg modified — drops one key).
- All 7 `.opt` paired stamps v3.27 -> v3.28; 6 `.opt` bodies byte-identical to v3.27 (only Beetle PCE Fast.opt modified — one value change).
- README.md: §1 FinalBurn Neo row Notes column updated. Drops "Run Ahead 2 + secondary instance (per FBN core maintainer)"; replaced with "Run Ahead 2 single-instance (per upstream FBN libretro README)". Keys count column 4 -> 3.
- README.md: §1 Beetle PCE Fast row Notes column updated. "4× streaming (revert per-game on Ys IV / Dracula X)" -> "2× streaming (per upstream advisory + community-standard reference)".
- README.md: §4 Frontend Override Keys table — `run_ahead_secondary_instance` row Purpose column updated. Prior "FBN `true` (per FBN core maintainer); Tier 2 explicit `false`; non-FBN Tier 1 inherits global `false`" replaced with "Tier 2 (Mupen) explicit `false`; all other cores inherit global `false` for single-instance runahead per upstream FBN libretro README".
- README.md: badge 3.27 -> 3.28.
- CHANGELOG.md: trim v3.23 entry per 5-release retention; retained entries are now v3.24-v3.28.
- cfg 22 -> 21, opt 19, cfg+opt 41 -> 40.

# 3.27 - 2026-05-09

- v3.27: README trim pass to vital information only; cfg + opt bodies byte-identical to v3.26 (paired stamps bumped).
- README.md: §1 Supported Cores Notes column compressed across 7 rows. Mupen Notes ~150 words -> ~40 words: keeps tvOS Metal-only stack rationale, P-core multithread pin, FrameDuping, 4P rumble parity; drops the inline list of frontend pins (`video_threaded`, `video_frame_delay_auto`, `rewind_enable`, `run_ahead_enabled`, `audio_latency`, `audio_sync`, `autosave_interval`) — those duplicate §4 verbatim. Other 6 rows compressed to single-sentence Notes.
- README.md: §4 Frontend Override Keys table Notes column simplified across 9 rows; trailing inherited-keys paragraph trimmed.
- README.md: §5 Shaders compressed from ~5 sentences to 2 (drop UI gating restatement and per-core file enumeration; keep zfast-crt + lcd-grid recommendation).
- README.md: §7 Manual Install: Per-Core Override Path collapsed. Prior 28-line full-tree example for all 7 cores -> single 4-line example tree for `Mesen/` plus a 1-line note enumerating the other 6 core directory names. Verify-loaded callout retained.
- README.md: §11 Versioning paragraph compressed from ~120 words to ~50 (drops historical re-alignment context, retention restated tersely).
- README.md: drops standalone "Apple TV / tvOS" subheader inside §7 (only one platform documented; subheader was filler).
- README.md: badge 3.26 -> 3.27.
- config/*.cfg: 7 files; bump header stamps and "paired with retroarch-appletv4k" stamps v3.26 -> v3.27. Bodies byte-identical to v3.26.
- config/*.opt: 7 files; unchanged (no version stamps; frontend-version-independent per v3.12 design).
- CHANGELOG.md: trim v3.22 entry per 5-release retention; retained entries are now v3.23-v3.27.
- Companion v3.27: retroarch.cfg byte-identical to v3.26 except header stamp (74 keys unchanged). README trim pass — §3 Storage Persistence 3 subsections collapsed to 1 paragraph; §6 Controllers Compatibility 6 rows -> 4 (Recommended / Excellent / Avoid grouping); §7 Hotkeys recommended-bindings table 8 rows -> 5 (paired actions on single rows); §7 Additional settings 40-row table replaced by 1-paragraph hardening-summary + 7-row "may want to flip" table; §10 Known Issues 7 rows -> 3 (closed entries removed; #18300, #14201, #16598, #14978 dropped; only #18286, #18447, #16685 retained as Open). §11 Setup Checklist dropped entirely (~50 lines; redundant with §1-§9). Subsequent sections renumbered §12 -> §11, §13 -> §12, §14 -> §13. README badge 3.26 -> 3.27. CHANGELOG trim v3.22 per matching 5-release retention.
- cfg 22, opt 19, cfg+opt 41 — unchanged.

# 3.26 - 2026-05-09

- mGBA.opt: 3 keys -> 1 key. Removed:
  - `mgba_interframe_blending = "OFF"` (matches upstream default at libretro/mgba `src/platform/libretro/libretro_core_options.h:208`; same class as v3.25 Genesis Plus GX `audio_filter` trim).
  - `mgba_audio_low_pass_filter = "disabled"` (matches upstream default at L223; same class).
  - Retained: `mgba_color_correction = "Auto"` (real flip from default `"OFF"`). Header rewritten to "1 key".
- FinalBurn Neo.cfg: header citation for `rewind_enable = "false"` reworded — `(#16374 savestate audio buzz with run-ahead)` -> `(#16374 savestate-size mismatch with rewind+runahead)` — to match the actual issue body at libretro/RetroArch#16374.
- Mupen64Plus-Next.cfg + .opt: header phrasing reframed for `cpucore = "cached_interpreter"`. "platform-forced stack" wording replaced with "tvOS Metal-only stack ... drift-guard against DYNAREC builds". tvOS App Store has `DYNAREC` undef, so `cached_interpreter` IS the upstream default per libretro/mupen64plus-libretro-nx `libretro/libretro_core_options.h:1391`. The other two stack members (`rdp-plugin = "angrylion"`, `rsp-plugin = "cxd4"`) remain real flips from defaults `"gliden64"` / `"hle"`.
- All 7 `.cfg` paired stamps v3.25 -> v3.26 (6 bodies byte-identical to v3.25; FinalBurn Neo.cfg and Mupen64Plus-Next.cfg additionally have the header rewords above).
- 6 `.opt` files unchanged; only `mGBA.opt` body edit (above).
- README.md: §1 mGBA row `.opt` count 3 -> 1; Notes column drops `interframe_blending` + `audio_low_pass_filter` annotations.
- README.md: §1 FBN row — `run_ahead_secondary_instance = "true"` citation reworded; `[#16374]` was the rewind+runahead conflict tracker, not the secondary-instance recommendation. Now framed as "per FBN core maintainer recommendation; see #16374 for the rewind+runahead savestate-size interaction".
- README.md: §1 Mupen row — `mupen64plus-cpucore = "cached_interpreter"` reframed as a drift-guard pin against `DYNAREC`-enabled builds (cite `libretro/libretro_core_options.h:1391`).
- README.md: §4 Frontend Override Keys — `run_ahead_secondary_instance` row drops `[#16374]` citation (kept on `rewind_enable` row where it actually applies).
- README.md: §8 Overclocking — rewritten with real upstream keys.
  - `mesen_overclock_rate` -> `mesen_overclock` (enum: None / Low / Medium / High / Very High; libretro/Mesen `Libretro/libretro_core_options.h:78`).
  - `snes9x_overclock` -> `snes9x_overclock_superfx` (50%-500%; libretro/snes9x `libretro/libretro_core_options.h:192`) + `snes9x_overclock_cycles` (disabled / light / compatible / max; L218).
  - Prior key names did not exist upstream — copy-paste guidance was silently inert. Added a real-keys reference table.
- README.md: §11 Versioning — changelog format documented as kernel.org style (`# Version - Date` + bullets). Prior "GNU ChangeLog style" wording dropped.
- README.md: badge 3.25 -> 3.26.
- CHANGELOG.md: trim v3.21 entry per 5-release retention; retained entries are now v3.22-v3.26. Format converted from GNU ChangeLog (`YYYY-MM-DD<SP><SP>Author`) to kernel.org style (`# Version - Date` + bullets) across all retained entries.
- Companion v3.26: retroarch.cfg byte-identical to v3.25 except header stamp (74 keys unchanged). README §7 / §9 / §10 / §13 surgical fixes (overclock key names, stale issue states, kernel.org-style versioning note); see retroarch-appletv4k v3.26 entry.
- cfg 22, opt 21 -> 19, cfg+opt 43 -> 41.
