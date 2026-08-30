# 4.3 - 2026-08-30

- v4.3: documentation and comment correction release. No key added, removed, or revalued; cfg 21, opt 18, cfg+opt 39 unchanged. Released in lockstep with companion retroarch-appletv4k v4.3.
- FinalBurn Neo.cfg / README.md: `rewind_enable = "false"` rationale corrected. Upstream FBNeo `src/burner/libretro/README.md` struck the #16374 note on 2026-05-12 ("this bug is seemingly fixed") and RetroArch #16374 is closed. The pin is value-identical to global `"false"`, so it is now documented as a drift-guard rather than a live workaround. Value unchanged.
- README.md / Mupen64Plus-Next.cfg: closed-issue citations marked. #14978 (`video_threaded`) and #18300 (`rewind_enable`) are closed upstream and both pins equal the global value; both are now labelled drift-guards, matching the treatment given #14201 at v4.2. #14201 itself is relabelled a real override retained as a regression guard, since global is `"true"` and Mupen pins `"false"`. Values unchanged.
- Mupen64Plus-Next.opt: `angrylion-multithread` comment corrected. The option sets the angrylion worker-thread count; it does not set CPU affinity, and tvOS exposes no P-core pinning to applications. Reframed as a worker budget sized to the 5-core A15 bin (2P+3E). Value `"2"` unchanged.
- Mupen64Plus-Next.cfg / .opt / README.md: "Metal-only (no GL/Vulkan)" reworded to "software stack". The tvOS build of mupen64plus-next does compile ParaLLEl-RDP and ParaLLEl-RSP (`Makefile` tvOS branch sets `HAVE_PARALLEL_RDP`, `HAVE_PARALLEL_RSP`, `HAVE_THR_AL`, `LLE`), so both appear in the core-option lists. They are unusable at runtime: RetroArch tvOS ships no Vulkan driver, and GLideN64 needs a GL context that `video_driver = "metal"` does not provide. Prior wording implied the plugins were absent from the build.
- README.md: Frontend override keys — the 21 per-core keys are now split into 14 real flips and 7 drift-guards set to the value they already inherit. Drift-guards: FBN `rewind_enable`; Mupen `video_threaded`, `audio_sync`, `audio_latency`, `run_ahead_enabled`, `run_ahead_secondary_instance`, `rewind_enable`.
- README.md: Frontend override keys — `video_scale_integer_scaling` row states the enum. `"1"` is overscale against upstream default `"0"` (underscale), and the key is inert unless global `video_scale_integer = "true"`.
- README.md: Overclocking — `snes9x_overclock_superfx` range corrected. The value list is discrete, not continuous: 50%-100% in 10% steps, then 150%-500% in 50% steps.
- README.md: Configuration — note added that `.cfg` headers are version-stamped and `.opt` headers deliberately are not (frontend-version-independent per v3.12 design).
- config/.gitkeep: removed. The directory has carried 14 tracked files since v3.x, so the placeholder is inert; Quick Start already states 14 files.
- README.md: the v4.0-v4.2 entries record a `paired` cross-link badge bump. No paired badge is present in the file — the header carries a single version badge. Recorded here rather than by editing the historical entries.
- config/*.cfg: 7 files; header and paired stamps v4.2 -> v4.3. Bodies byte-identical to v4.2 except comment text in FinalBurn Neo.cfg and Mupen64Plus-Next.cfg.
- config/*.opt: no version stamps; comment-only edits to Mupen64Plus-Next.opt, other 6 byte-identical to v4.2.
- README.md: version badge 4.2 -> 4.3.
- Companion v4.3: retroarch.cfg byte-identical to v4.2 except header stamp (74 keys unchanged).
- CHANGELOG.md: trim v3.27 entry per 5-release retention; retained entries are now v3.28 + v4.0-v4.3.

# 4.2 - 2026-07-26

- v4.2: documentation-only release. No key added, removed, or revalued; cfg 21, opt 18, cfg+opt 39 unchanged.
- Mupen64Plus-Next.cfg: `audio_latency` comment corrected. Companion v4.1 raised global `audio_latency "48" -> "64"`, so the pin no longer sits +16 ms above global — it now equals it. Reframed as an explicit Tier 2 pin held against global drift, matching the treatment of `audio_sync`, `video_threaded`, `run_ahead_secondary_instance`, and `rewind_enable`. Value `"64"` unchanged.
- README.md: Frontend override keys — `audio_latency` row rewritten for the same reason.
- README.md: Layout — tvOS path mapping corrected. `Documents/RetroArch/` is exposed as `/` by the web interface / WebDAV, not as `config/`; per-core directories are therefore at `/config/<core_name>/`. Prior wording implied `config/config/<core_name>/`.
- README.md / Mupen64Plus-Next.cfg: `run_ahead_enabled = "false"` rationale corrected. The prior "HW-GL serialize breakage" note contradicted the documented Metal-only, software-RDP stack and the repo's own per-game run-ahead example. Reframed as a per-frame savestate cost decision with per-game opt-in. Value unchanged.
- README.md / FinalBurn Neo.opt: menu path corrected to Quick Menu -> Core Options, matching the RetroArch v1.22.0 label and the Overclocking section.
- README.md: Frontend override keys — the inherited-keys note referenced `video_shader`, which is not a config key in RetroArch v1.22.0. Reworded to state that no `.cfg` sets a shader preset and that presets are assigned per-core via Save Core Preset.
- Beetle PCE Fast.opt: CD comment trimmed. The 4 GB precache rationale is stated in the header line; the inline restatement is dropped, keeping `cdspeed` guidance and the per-game `cdimagecache` opt-in.
- config/*.cfg: 7 files; header and paired stamps v4.1 -> v4.2. Bodies byte-identical to v4.1 except Mupen64Plus-Next.cfg (comment only).
- config/*.opt: no version stamps (frontend-version-independent per v3.12 design); comment-only edits to Beetle PCE Fast.opt and FinalBurn Neo.opt.
- README.md: version badge 4.1 -> 4.2; paired badge `retroarch--appletv4k v4.1` -> `v4.2`.
- Companion v4.2: retroarch.cfg byte-identical to v4.1 except header stamp (74 keys unchanged).
- CHANGELOG.md: trim v3.26 entry per 5-release retention; retained entries are now v3.27-v3.28 + v4.0-v4.2.

# 4.1 - 2026-07-05

- v4.1: Beetle PCE Fast.opt drops `pce_fast_cdimagecache = "enabled"`, reverting to upstream default `"disabled"`. opt 3 keys -> 2 keys.
- Rationale: full CD-image RAM precache is imprudent as a global default on the 4 GB shared-RAM, fanless Apple TV 4K 3rd Gen (binned A15, 2P+3E). A multi-hundred-MB footprint contends with tvOS and risks memory-pressure stalls. Seek-latency relief retained as a per-game opt-in, consistent with the per-game policy for hardware-dependent keys.
- Beetle PCE Fast.opt: header rewritten for 2 keys and per-game precache guidance. `cdspeed = "2"` and `nospritelimit = "enabled"` unchanged (real flips from defaults `"1"` / `"disabled"`).
- README.md: Supported Cores — Beetle PCE Fast `.opt` count 3 -> 2; Notes drop "CD precache", add the per-game precache rationale.
- README.md: version badge 4.0 -> 4.1; paired badge `retroarch--appletv4k v4.0` -> `v4.1`.
- config/*.cfg: 7 files; header and paired stamps v4.0 -> v4.1. Bodies byte-identical to v4.0.
- Companion v4.1: retroarch.cfg `audio_latency "48" -> "64"`, matching upstream `config.def.h` DEFAULT_OUT_LATENCY 64 on the non-Android branch; 74 keys otherwise unchanged.
- CHANGELOG.md: trim v3.25 entry per 5-release retention.
- cfg 21, opt 19 -> 18, cfg+opt 40 -> 39.

# 4.0 - 2026-05-16

- v4.0: MAJOR bump — README restructured to ry-install style (breaking anchor schema); 7 `.cfg` paired stamps v3.28 -> v4.0; 7 `.opt` byte-identical.
- README.md: **BREAKING** anchor schema change — slugs drop the leading `N-` prefix (`#1-supported-cores` -> `#supported-cores`, `#7-manual-install-per-core-override-path` -> `#layout`). Inbound links to old anchors will 404.
- README.md: numbered sections and all `§N` cross-references retired; `## Table of Contents` -> `## Contents`, numbered list -> bulleted.
- README.md: section consolidation — File Structure + Installation + Manual Install collapsed into `## Layout`; File Separation absorbed into `## Configuration`. Section count 12 -> 9.
- README.md: `Zip contents (flat)`, `Frontend override keys`, and `Shaders` folded into default-collapsed `<details>` blocks.
- README.md: GitHub admonitions replace prose warnings — `> [!IMPORTANT]` in Quick Start (per-core-path miss silently disables Tier 1 run-ahead); `> [!WARNING]` in Configuration (mixing `.cfg` / `.opt` fails silently).
- README.md: header gains a `paired` cross-link badge to the companion repo; License section adopts `MIT © 2026 Ryan Musante`.
- README.md: badge 3.28 -> 4.0; paired badge v3.28 -> v4.0. Byte size 7792 -> 6914 (-11.3%); line count 142 -> 173.
- config/*.cfg: 7 files; header and paired stamps v3.28 -> v4.0. Bodies byte-identical to v3.28.
- config/*.opt: 7 files unchanged; no version stamps. The v3.28 reference to ".opt paired stamps" was loose wording.
- CHANGELOG.md: trim v3.24 entry per 5-release retention.
- cfg 21, opt 19, cfg+opt 40 — unchanged.

# 3.28 - 2026-05-10

- v3.28: FinalBurn Neo.cfg drops the `run_ahead_secondary_instance` pin; Beetle PCE Fast.opt rebases `pce_fast_cdspeed`.
- FinalBurn Neo.cfg: drop `run_ahead_secondary_instance = "true"`. Upstream FBN libretro README recommends runahead single instance and preemptive frames; second-instance support is not guaranteed. The prior "per FBN core maintainer" attribution conflicted with that guidance. FBN now inherits global `"false"`. cfg 4 keys -> 3 keys.
- Beetle PCE Fast.opt: `pce_fast_cdspeed "4" -> "2"`. Upstream advises higher values can break some games; `"2"` matches community-standard reference. Per-game `"4"` remains available. opt 3 keys unchanged.
- README.md: FinalBurn Neo row — "Run Ahead 2 + secondary instance" -> "Run Ahead 2 single-instance (per upstream FBN libretro README)"; keys 4 -> 3.
- README.md: Beetle PCE Fast row — "4× streaming (revert per-game on Ys IV / Dracula X)" -> "2× streaming".
- README.md: Frontend Override Keys — `run_ahead_secondary_instance` Purpose rewritten to "Tier 2 (Mupen) explicit `false`; all other cores inherit global `false`".
- README.md: badge 3.27 -> 3.28.
- CHANGELOG.md: trim v3.23 entry per 5-release retention.
- cfg 22 -> 21, opt 19, cfg+opt 41 -> 40.
