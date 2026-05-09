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

# 3.23 - 2026-04-25

- v3.23: paired README de-versioning pass; 0 file-content changes.
- config/*.cfg: bump header stamps and "paired with retroarch-appletv4k" stamps v3.22 -> v3.23 (7 surviving files; bodies byte-identical to v3.22).
- config/Mupen64Plus-Next.cfg: header drops "(v3.3 stutter mitigations)" inline historical annotation; key body byte-identical.
- config/Mupen64Plus-Next.opt: header drops "(v3.3 Angrylion-MT heterogeneous-ARM tune)" and "(v3.21 P2-P4 parity for Mario Kart 64, Mario Party 1-3, GoldenEye, Perfect Dark, ProAm 64)" inline historical annotations; pak2/3/4 inline comment drops "v3.21:" prefix; key body byte-identical.
- config/*.opt (other 6): unchanged.
- README.md: §1 supported cores table — Genesis Plus GX row drops "(v3.1 off; v3.2 enum fix)" annotation on `audio_filter`; mGBA row drops "(v3.9 enum fix — prior releases used 'disabled' which is the UI label, not a valid key; core fell through to default 'OFF' silently. Same class of bug as the v3.2 `color_correction` fix...)" and "(v3.2 enum fix — now actually applies)" annotations; Mupen row drops "v3.21" / "v3.3" / "v3.9" annotations.
- README.md: §4 Frontend Override Keys table de-versioned across 5 row Notes columns (`run_ahead_secondary_instance`, `audio_latency`, `audio_sync`, `autosave_interval`, `video_frame_delay_auto`) plus the closing inherited-keys paragraph (drops "v3.11" on `run_ahead_frames`).
- README.md: §11 Versioning drops historical re-alignment example "(e.g. v3.0 re-aligned the two repos after they had evolved independently at v2.95 / v1.57)".
- README.md: badge 3.22 -> 3.23.
- CHANGELOG.md: trim v3.18 entry per 5-release retention; retained entries are now v3.19-v3.23.
- Companion v3.23: retroarch.cfg byte-identical to v3.22 except header stamp (73 keys unchanged). README §7 Additional settings preamble + table de-versioned across 14 rows; §7 Video table de-versioned across 5 rows; §9 Mupen Tier 2 row de-versioned; §10 Known Issues #4 de-versioned; §13 Versioning drops historical re-alignment example. Editorial rule established: README narrative no longer carries per-version history; CHANGELOG is the sole record of what changed when.
- cfg 22, opt 25, cfg+opt 47 — unchanged.

# 3.22 - 2026-04-25

- v3.22: PlayStation 1 / PCSX-ReARMed core retired; cores 8 -> 7. cfg+opt 61 -> 47.
- config/PCSX-ReARMed.cfg deleted (-8 keys).
- config/PCSX-ReARMed.opt deleted (-6 keys).
- config/*.cfg: bump header stamps and "paired with retroarch-appletv4k" stamps v3.21 -> v3.22 (7 surviving files; bodies byte-identical to v3.21).
- config/*.opt: unchanged (no version stamps; frontend-version-independent per v3.12 design; 7 surviving files).
- README.md: cores badge 8 -> 7; version badge 3.21 -> 3.22.
- README.md: §1 supported cores table drops PCSX-ReARMed row; Mupen64Plus-Next row already updated v3.21 for `pak1/2/3/4 = "rumble"` 4P parity (opt count 7 -> 10 retained).
- README.md: §2 file structure tree drops PCSX-ReARMed.cfg/.opt entries; ship-count line "8 `.cfg` and 8 `.opt`" -> "7 `.cfg` and 7 `.opt`".
- README.md: §4 Frontend Override Keys table de-PCSX'd across 9 rows — `run_ahead_enabled`, `run_ahead_secondary_instance`, `audio_latency` (now Mupen-only `64`), `audio_sync` (now Mupen-only mirror of global), `autosave_interval` (now Mupen-only pin), `video_scale_integer_scaling` (drops "+ PCSX" and PS1 variable-width caveat), `video_frame_delay_auto` (drops "+ PCSX inherits"), `rewind_enable` (drops PCSX from list). `video_scale_integer` row dropped entirely — was PCSX-ReARMed-only drift-guard mirror; no surviving core needs it.
- README.md: §7 install tree drops PCSX-ReARMed/ directory block.
- README.md: §8 Overclocking drops the `pcsx_rearmed_psxclock` "exception" paragraph entirely; remaining text covers `mesen_overclock_rate` + `snes9x_overclock` per-game guidance.
- CHANGELOG.md: trim v3.16 + v3.17 entries; retained entries are now v3.18-v3.22 (5-release retention).
- Companion v3.22: retroarch.cfg byte-identical to v3.21 except header stamp (73 keys unchanged); README §1 BIOS list, §4 filesystem layout, §5 ROM/BIOS tables, §7 video/audio rows, §8 shader recommendations table (9 -> 3 primary CRT shaders), §9 Tier 2 supported systems table all de-PCSX'd; §8 Recommended presets retains `crt-easymode` + `crt-aperture` + `crt-geom` only.
- cfg 22, opt 25, cfg+opt 47.
