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

# 3.25 - 2026-05-02

- v3.25: paired companion default-matching-key trim; 4 inert/drift-guard keys removed across 2 `.opt` files; 7 `.cfg` paired stamps.
- config/Genesis Plus GX.opt: 6 keys -> 3 keys. Removed:
  - `genesis_plus_gx_ym2612 = "mame (ym2612)"` (matches upstream default at libretro/Genesis-Plus-GX `libretro_core_options.h:444`; drift-guard pin).
  - `genesis_plus_gx_audio_filter = "disabled"` (matches upstream default at L475; misleading "off for CPU headroom" annotation — no headroom gained vs default).
  - `genesis_plus_gx_render = "single field"` (matches upstream default at L347; drift-guard pin).
  - Retained: `no_sprite_limit` (real flip), `system_bram` + `cart_bram` (real flips, per-game BRAM). Header rewritten to reflect 3 remaining keys.
- config/Mupen64Plus-Next.opt: 10 keys -> 9 keys. Removed:
  - `mupen64plus-43screensize = "320x240"` — under HAVE_THR_AL angrylion path, value is read at libretro/mupen64plus-libretro-nx `libretro/libretro.c:1348` then immediately overwritten at L1370-1371 by `if(current_rdp_type == RDP_PLUGIN_ANGRYLION)` forced override to `retro_screen_width=640, retro_screen_height=480, retro_screen_aspect=4.0/3.0, AspectRatio=1`. Functionally inert under the platform-forced `rdp-plugin=angrylion` stack. Header drops 43screensize reference; remaining 9 keys retain.
- config/*.cfg: bump header stamps and "paired with retroarch-appletv4k" stamps v3.24 -> v3.25 (7 files; 6 bodies byte-identical to v3.24; Mupen64Plus-Next.cfg additionally trims one verbose multi-clause audio comment line to single-line per editorial directive).
- config/*.opt: 3 files (Mesen / Mupen / mGBA) trim verbose multi-clause comments to single-line and drop legacy `(v3.X)` historical annotations per established v3.23 editorial rule. Beetle PCE Fast.opt / FinalBurn Neo.opt / Snes9x.opt unchanged — already minimal at v3.24.
- README.md: §1 Supported Cores table — Genesis Plus GX `.opt` count 6 -> 3 (notes column rewritten; drift-guard rationales removed where keys removed); Mupen64Plus-Next `.opt` count 10 -> 9 (notes column drops `43screensize` reference).
- README.md: §2 File Structure tree unchanged (file paths same; only contents trimmed).
- README.md: §4 Frontend Override Keys table unchanged (only `.cfg` keys; `.opt` changes don't affect this section).
- README.md: badge 3.24 -> 3.25.
- CHANGELOG.md: trim v3.20 entry per 5-release retention; retained entries are now v3.21-v3.25.
- Companion v3.25: retroarch.cfg `audio_resampler_quality "2" -> "3"` (real flip from tvOS LOWER default to NORMAL; SINC vs linear; A15 sub-1% CPU); + `input_max_users = "4"` (multi-player phantom-slot cap; #16685 cross-reference; 4P pak alignment); key count 73 -> 74. README intro "73-key" -> "74-key"; §7 Audio resampler row rewritten; §7 Input + new `input_max_users` row; badge 3.24 -> 3.25; CHANGELOG trim v3.20 per matching 5-release retention.
- cfg 22, opt 25 -> 21, cfg+opt 47 -> 43.

# 3.24 - 2026-04-25

- v3.24: paired README §5 shader-recommendation retarget; 0 file-content changes.
- config/*.cfg: bump header stamps and "paired with retroarch-appletv4k" stamps v3.23 -> v3.24 (7 files; bodies byte-identical to v3.23).
- config/*.opt: unchanged (no version stamps; frontend-version-independent per v3.12 design; 7 files).
- README.md: §5 Shaders retargets recommended-starting-point reference from `crt-easymode.slangp` to `crt/zfast-crt.slangp` (single-pass, integer-scale safe with no shader geometry, designed for low-end GPUs). Drops the prior "cleaner 4K phosphor mask than crt-aperture" comparative clause (no longer applicable — crt-aperture not in current lineup). Path corrected from `../shaders/shaders_slang/handheld/lcd-grid-v2.slangp` to `handheld/lcd-grid-v2.slangp` (matches companion §8 reference style and RetroArch UI navigation context). mGBA LCD recommendation target unchanged.
- README.md: §5 fixes stale "8 per-core `.cfg` files" -> "7 per-core `.cfg` files". v3.22 dropped PCSX-ReARMed core (cores 8 -> 7); §1 supported cores table and §2 file structure tree were updated in that pass but §5 prose was missed.
- README.md: badge 3.23 -> 3.24.
- CHANGELOG.md: trim v3.19 entry per 5-release retention; retained entries are now v3.20-v3.24.
- Companion v3.24: retroarch.cfg byte-identical to v3.23 except header stamp (73 keys unchanged). README intro stale "77-key" -> "73-key" fix (drift since v3.19; intro paragraph was missed in each subsequent README pass). README §8 Recommended presets table 3 -> 2 rows (`crt/zfast-crt.slangp` Minimal cost as sole CRT recommendation + `handheld/lcd-grid-v2.slangp` Minimal cost as sole mGBA-LCD recommendation; replaces `crt-easymode` / `crt-aperture` / `crt-geom`). §8 parameter callout retargeted with verified zfast-crt impl parameters (BLURSCALEX, LOWLUMSCAN, HILUMSCAN, BRIGHTBOOST, MASK_DARK, MASK_FADE — sourced from `crt/shaders/zfast_crt/zfast_crt_impl.inc` on libretro/slang-shaders master). §8 "Integer Scaling Conflict" callout dropped (no longer relevant to single-pass / integer-scale-safe lineup; multi-pass shaders covered by surviving "Avoid on Apple TV" line). §8 "Handheld note" + step 3 cross-reference + "Applying ... per-core" sentence retargeted from `crt-easymode` to `zfast-crt`.
- cfg 22, opt 25, cfg+opt 47 — unchanged.

