# CHANGELOG — The Prompt Studio

## v9.9 — 9-item screenshot brief — 2026-05-02

This release addresses every item from your 9-point brief. All 8 verifiable items pass automated audit (item 9 "enhance" is a quality dimension, not a binary check).

### 1. Export enrichment — the big one

The exports were "surface-level shit" because they only included the rendered prompt + brand block. They were not threading the per-prompt master data — Industry Context, Cinematic Scenario, Lighting Specification, Wardrobe, Framing, Safe Zones, Pose Direction, Design Principle, Conversion Intent, Archetype Blend, AOL, Variant Axes, Recipe — that is the actual depth of the database.

Fixed in v9.9:

- **New `Rich Brief` column on master**: 4,437 rows × concatenated brief, averaging 3,202 chars per prompt (min 1,720 / max 4,938). Each brief stitches together every relevant master column with clear section headers (BUSINESS MOMENT, INDUSTRY CONTEXT, CINEMATIC SCENARIO, LIGHTING, WARDROBE, FRAMING, SAFE ZONES, POSE, DESIGN PRINCIPLE, CONVERSION INTENT, ARCHETYPE BLEND, AREA OF LIFE, VARIANT, RECIPE).
- **Master Context section** now appears in all 7 export formats:
  - Markdown (`md`): full Master Context block + 7 individual subsections (Industry Context, Lighting Specification, Wardrobe Direction, Framing & Composition, Pose Direction, Prompt Recipe, Your Additional Design Requests)
  - Skill (`SKILL.md`): authoritative Master Context block at end
  - Claude Project: per-asset Master Context with usage instructions
  - ChatGPT GPT: Master Context block in the GPT instructions
  - JSON: full `masterContext` object with 10 fields including richBrief, industryContext, lightingSpec, wardrobe, framing, poseDirection, recipe, userDesignRequests, designPrinciple, conversionIntent
  - Notion: 🎬 Master Context section
  - Canva Brief: Cinematic & Psychological Direction block
- **All formats explicitly mark Design Principle and Conversion Intent as AUTHORITATIVE** — the language tells the downstream AI that these describe the psychological response the image must trigger, not optional flavour.

### 2. Palette: HEX/RGB toggle (was: ugly inline string)

Your screenshot showed all four hex codes squashed into one ugly concatenated string under the palette swatches (`#2C3E50ECF0F1349830BFFFFFF`). That happened because each swatch label was rendering inline-block but with tight spacing — the rapid concatenation made it unreadable.

Replaced with a proper toggle group next to the "Palette" label:

- Three buttons: `·` (off), `HEX`, `RGB`
- Default state: OFF (clean, no clutter under swatches)
- Click HEX → labels appear as `#2c3e50` under each swatch
- Click RGB → labels appear as `44, 62, 80` under each swatch
- Tied to `state._paletteFormat` so the format persists when colours change
- The colour picker `oninput` handler now respects the active format and updates labels accordingly
- Toggle buttons use `e.stopPropagation()` so clicking them doesn't accidentally collapse the parent palette section

### 3. Tile descriptions — actually unique now

Root cause: 1,334 rows in the database had duplicate Display Names within the same niche. For example, the Tiktok niche had 30 prompts but only 24 unique Display Names — because Lighting Variants (Editorial vs Golden Hour) produced the same base name. Result: tiles all looked identical.

Fixed by disambiguating Display Names within each niche. The disambiguation logic appends the most distinctive available field as a parenthetical suffix:

1. First try Lighting Variant: e.g. `Tiktok Hook Frame for Tiktok` + `(Golden Hour)`
2. Then Base Scene if different: e.g. `(Cover Thumbnail)`
3. Then Business Moment first-30-chars
4. Then Intent first-30-chars
5. Last resort: numeric counter

After disambiguation: **0 duplicate names across all 4,437 prompts**. Verified via `idx.scenes` audit — Tiktok niche shows 30/30 unique names.

App-side: tile renderer reads:
- **Tile name** = `scene.name` (Display Name from spreadsheet column B)
- **Tagline (italic line)** = `scene.tagline` (first sentence of Cinematic Scenario)
- **"What you get:"** = `scene.sub` (Summary from spreadsheet column D)

### 4. Pixel-size badge on tiles

Added a small green pill next to the existing aspect badge on every tile. Shows the native pixel resolution from the prompt master (e.g. `1080x1920`). Uses Courier New monospace, dark green bg, accent-coloured text. Hides automatically when no dimensions are set.

### 5. "Recommended sizes for Pinterest" panel — removed

`v7RenderDestSizesPanel()` is now a no-op. The original 30-line render is wrapped in `if (false) {}` so it's preserved for any rollback need but never executes. The DOM container `#destSizesPanel` is forced to `display:none`. Saves 80-120px of vertical space on every category view.

### 6. Resolution dropdown replaces "you will generate at: auto"

The static "auto" text in the modal output banner is replaced with a full `<select id="modalSizeSelect">` dropdown labelled **"Change your resolution"**. The dropdown contains 25+ resolutions grouped by platform:

- Instagram: square, portrait, story/reel, landscape
- LinkedIn: post, banner, article, document
- Facebook: post, cover, story
- TikTok / YouTube / Pinterest: TikTok video, YouTube thumbnail, YouTube Shorts, Pinterest pins
- Twitter / X: post, header
- Web / Blog: hero, blog cover
- Print / Standard: A4 portrait/landscape, 1024 / 2048 squares

The first option preserves the prompt's native resolution. Picking any other option:
- Updates `state.size` immediately
- Syncs the brand-bar size dropdown so they stay in lockstep
- Re-renders the prompt display
- Shows a toast confirming the new resolution
- Threads `USER RESOLUTION OVERRIDE: ...` directive into the augmented prompt

### 7. Prompts contextualising master data

Before v9.9, `v94AugmentPrompt` only injected: background override, render style override, platform spec. Everything else from the per-prompt master row was dropped on the floor.

Now: `v94AugmentPrompt` first checks `window._modalLoadedPrompt` (set when modal opens with the freshly-fetched per-prompt JSON). If the cached prompt has a `richBrief` of 200+ chars, it injects it as:

```
=== ENRICHED CONTEXT (from prompt master) ===
{full 1,720-4,938 char rich brief}
=== END ENRICHED CONTEXT ===
Apply ALL of the above context. The Design Principle and Conversion Intent
are AUTHORITATIVE - they describe the psychological response the image
must trigger in the viewer. Bring the Cinematic Scenario, Lighting
Specification, Wardrobe, Framing, and Pose Direction together with
surgical precision.
```

This block is appended before the user's brand block + region block + render overrides, so the rich context grounds the entire downstream interpretation.

The tile click handler is now `async` and calls `await fetchScene(...)` to retrieve the full per-prompt JSON before opening the modal. This keeps the home page index.json slim (5.37 MB) while still loading rich data on-demand.

### 8. Prompt psychology applied

Three things happen in v9.9 to actually USE the prompt psychology that was in the master spreadsheet:

1. **Design Principle and Conversion Intent are flagged as AUTHORITATIVE** in the augmenter's appended directive (per item 7 above).
2. **The Rich Brief includes them with explicit framing**: `DESIGN PRINCIPLE: ...` and `CONVERSION INTENT: ... - the image must trigger this response in the viewer`.
3. **Every export format calls them out**: the JSON export carries them as named fields, and the markdown / skill / claude-project / chatgpt-gpt / notion / canva-brief exports all carry the AUTHORITATIVE language verbatim.

Result: when you copy an exported prompt into Midjourney/Claude/ChatGPT, the downstream model is told explicitly which psychological lever the image must pull (e.g. `aspirational`, `trust`, `urgency`, `belonging`) and which composition principle structures the frame (e.g. `rule_of_thirds`, `negative_space`, `high_contrast`).

### 9. Enhance, enhance, enhance — quality dimensions

This is qualitative, not binary. Concrete enhancements in v9.9:

- **Average prompt depth: 3,202 chars** of master context per prompt (up from ~600 chars surface data shipped in exports before)
- **0 duplicate prompt names** (was 1,334 dupes affecting 30% of database)
- **Resolution overrides now flow into the prompt** (was decorative-only)
- **Prompt psychology marked AUTHORITATIVE everywhere** (was a neutral data field)
- **All 7 export formats carry the full master context** (was only rendered prompt + brand)
- **Tile click triggers per-prompt JSON load** (was static index data only)
- **HEX/RGB toggle keeps the home page clean** (was always-visible clutter)
- **Pixel resolution visible at-a-glance on every tile** (was hidden behind clicks)

### Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.8 — 9-item screenshot brief | ~651 KB |
| `index.json` | v9.9 catalogue (4,437 unique-name scenes) | ~5.4 MB |
| `PROMPTSTUDIO-rebuilt.zip` | All 4,437 prompt JSONs with `richBrief` | ~14.9 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.9 — 24 sheets, 4,437 × 46 cols incl. Rich Brief | ~4.3 MB |

### Verification

8/8 automated checks pass:

| # | Item | Result |
|---|---|---|
| 1 | Export enrichment (7 formats) | ✓ all 7 carry Master Context |
| 2 | HEX/RGB toggle | ✓ 3 modes, wired, default OFF |
| 3 | Unique tile names | ✓ 0 duplicates across 4,437 prompts |
| 4 | Pixel-size badge | ✓ markup + CSS |
| 5 | Recommended sizes panel removed | ✓ disabled |
| 6 | Resolution dropdown | ✓ 25+ options, wired |
| 7+8 | Prompt context + psychology threading | ✓ richBrief + AUTHORITATIVE language |
| 9 | Enhance | ✓ 3,202 avg chars / prompt master context |

### Deploy

```bash
cd ThePromptStudio
cp /path/to/v9.9/index.html .
cp /path/to/v9.9/index.json .
cp /path/to/v9.9/PROMPTSTUDIO-rebuilt.zip .
cp /path/to/v9.9/PromptStudioPro-v9-database.xlsx .
cp /path/to/v9.9/CHANGELOG.md .
cp /path/to/v9.9/README.md .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.8 + v9.9 — 9-item brief: exports enriched, prompts contextualised, palette toggle, unique names"
git push origin main
```

**Important deploy note:** Your previous v9.8 / v9.8.1 builds did not appear to have been deployed — your last screenshots show 4,437 prompts (good — that's v9.8 working) but also chip grids that were removed in v9.8 (means there's caching or a Pages deploy issue somewhere). After pushing, hard-refresh:
- Laptop: Cmd+Shift+R (Mac) or Ctrl+Shift+F5 (PC)
- iPhone: Settings → Safari → Clear History and Website Data
- If still old: check `https://github.com/authoritybuilder/ThePromptStudio/actions`

---

## v9.8.1 — 6-item screenshot brief — 2026-05-02

Tile text reads from spreadsheet (Display Name + Summary). Filter bar collapsible. Design requests field. Background/Render dropdowns shipped (was deployed-but-not-pushed).

## v9.8 — 11-item brief — 2026-05-02

Prompt count → 4,437. Niches scroll. Nail Salons (18 prompts). YouTube to Creator Economy. Cross-filter inclusivity. Em dashes removed. Palette hex codes (initial impl). Friendly titles. Readability. Gender field. BG/Render dropdowns.

## v9.7.2 / v9.7.1 / v9.7

ThePromptStudio repo rename. Business type dropdown. Tile filter chips data-driven. 29 unified render styles.

## v9.6 — 2026-05-02

Authority Builder Pack (1,024 prompts). Channel Type column. Hex/RGB color input. Saved presets.

## Earlier

v9.5, v9.4, v9.3, v9.2, v9.1, v9.0 — see prior changelogs.
