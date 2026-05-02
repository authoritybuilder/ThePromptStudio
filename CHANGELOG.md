# CHANGELOG — The Prompt Studio

## v9.7 Database + v8.9.3 App — Unified Render Styles + Tile Filters + Export Pipeline Fix — 2026-05-02

This release closes out the last three items from the 11-item brief: **Item 3** (cartoon styles alignment), **Item 8** (more filter dimensions), **Item 10** (export pipeline verification).

### Item 3 — Unified render style taxonomy

**The problem:** The wizard had 20 cartoon style IDs in kebab-case (`pixar`, `dreamworks`, `anime`, `watercolour`, etc.). The modal's V94_RENDER_STYLES had 12 different IDs in snake_case (`photoreal`, `cartoon_modern`, `anime_manga`, `watercolor_painted`, etc.). Different ontologies, different naming conventions, no sync between them. A user picking "Pixar" in the wizard would open a prompt and see "3D Pixar-Style" highlighted — close, but the wizard's choice never actually flowed through.

**The fix:** Unified taxonomy of **29 styles in 7 families**, all using kebab-case:

- **photoreal (4):** hyper-realistic, editorial-photo, documentary-photo, slight-retouch
- **3d (5):** pixar, dreamworks, lowpoly-3d, isometric, claymation
- **anime (3):** anime, anime-modern, manga-bw
- **illustration (6):** flat-vector, editorial-illustration, storybook, line-art, minimalist-line, ink-sketch
- **painted (2):** watercolour, oil-painting
- **stylised (8):** comic, pop-art, collage-mixed, papercraft, risograph, retro-vintage, pixel-art, cartoon-modern
- **2d-anim (1):** disney-classic

What changed in the app:

- `V94_RENDER_STYLES` constant replaced with the 29-entry unified set
- Wizard's `cartoonStyles` array updated to use the same kebab-case IDs (now 25 non-photoreal entries)
- Modal renders styles **grouped by family** with section labels — "📸 Photoreal", "🧸 3D / CGI", "🌸 Anime / Manga", etc. — so 29 options stay scannable
- New CSS: `.v94-render-family-label`, `.v94-render-family-row` for the grouped layout
- Wizard's `state.designStyle` and `state.cartoonStyle` now flow through to the modal's default render style. Pick "Pixar" in the wizard → open any prompt → Pixar is the active chip
- Backward-compat layer: `V97_RENDER_OLD_TO_NEW` mapping + `v97MigrateRenderKey()` helper translates old IDs at runtime
- `v94AugmentPrompt()` and the modal's `selectedRender` line both call the migration helper, so existing 4,419 prompt JSONs and any saved presets keep working

What changed in the data:

- Master `Render Style` column migrated: 3,087 `photoreal` → `hyper-realistic`, 926 `photoreal_editorial` → `editorial-photo`, 323 `photoreal_documentary` → `documentary-photo`, 45 `illustration_editorial` → `editorial-illustration`, 19 `illustration_flat` → `flat-vector`, 18 `minimalist_line` → `minimalist-line`, 1 `collage_mixed` → `collage-mixed`. Total: 4,419 rows migrated.
- All 4,419 prompt JSONs in the zip have `v94_render` translated to the new keys
- `index.json` scenes all have unified `v94_render` values

### Item 8 — Tile-level filter chips

Added a horizontal filter chip bar above the tile grid with three filter dimensions:

- **Aspect:** All / 16:9 / 4:5 / 1:1 / 9:16
- **Background:** All + top 6 styles (Lifestyle Environment, Studio White, Studio Black, Solid Brand Color, Subtle Gradient, Bold Gradient)
- **Render:** All + top 6 styles (Hyper-Realistic, Editorial Photo, Documentary, Pixar, Flat Vector, Watercolour)

Each chip toggles a filter on/off. Filters compose (multi-dimensional). A "Clear" button on the right resets all three. Empty state shows a "Clear filters" link inside the empty message.

What changed:

- New state field: `state.tileFilters = { aspect: 'All', background: 'All', render: 'All' }`
- New function `renderTileFilters()` builds the chips and wires click handlers
- `renderTilesList()` now applies the three filters before rendering
- `renderAllNav()` calls `renderTileFilters()` so chip state stays in sync with filtered scenes
- Mobile-responsive: chips collapse to vertical stacks under 640px width
- Render filter uses `v97MigrateRenderKey()` so it works correctly during the transition period when scenes might have old or new keys

### Item 10 — Export pipeline verification + fix

I audited all 8 export formats and 4 prompt-text paths. Found two real bugs:

**Bug 1:** `buildPrompt(scene)` (used by the brand-bar's "Copy & Open AI" button when no scene-specific export is active) called `v3GetEnhancedPrompt` directly without piping through `v94AugmentPrompt`. So if a user opened a prompt, picked a custom background and render style in the modal, then closed the modal and used the brand-bar copy button, **their customisations were silently dropped**.

**Bug 2:** `buildExport`'s `v5RenderedPrompt` (used by the MD format download) had the same problem — `v3GetEnhancedPrompt` only, no `v94AugmentPrompt`. So downloading a `.md` file would give you the original prompt without the modal overrides.

**The fix:** Both call sites now pipe through `v94AugmentPrompt(text, scene)` after `v3GetEnhancedPrompt`. Modal customisations now flow into:

- Modal "Copy" / "Copy & Open AI" buttons (already correct via `getPromptText` — verified)
- MD format download (fixed)
- Brand-bar Copy button (fixed)
- Modal export bar (already correct via `getPromptText` — verified)

The other 7 export formats (`skill`, `claude-project`, `chatgpt-gpt`, `json`, `notion`, `skool`, `canva-brief`) build brand context bundles, not single prompts. They're correct as-is — they don't need a single augmented prompt because they're brand-instruction packages applied across many prompts.

### Quality stats

| Metric | v9.6 | v9.7 |
|---|---|---|
| Total prompts | 4,419 | 4,419 (unchanged) |
| Render style IDs across UI | 32 different (20 wizard + 12 modal) | **29 unified** |
| Tile-level filter dimensions | 0 | **3** (aspect / bg / render) |
| Export paths piping through v94Augment | 2 | **5+** |
| Min Claude prompt length | 1,544 | 1,544 |
| Median Claude prompt length | 1,934 | 1,934 |

### Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.3 — unified render styles, tile filters, export fix | 623 KB |
| `index.json` | v9.7 catalogue (4,419 scenes, migrated render keys) | 3.5 MB |
| `PROMPTSTUDIO-rebuilt.zip` | All 4,419 prompt JSONs (render keys migrated) | 15.8 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.7 — 24 sheets, 4,419 × 45 cols | 3.94 MB |

### 11-item brief — all complete

| # | Item | Status |
|---|---|---|
| 1 | Pixel/platform terminology consistency | ✓ v9.6 |
| 2 | Hex / RGB color input | ✓ v9.6 |
| 3 | Cartoon styles align with popup tiles | ✓ **v9.7** |
| 4 | UI/wizard/tile terminology cross-reference | ✓ v9.6 |
| 5 | Reduce wasted space in popup tiles | ✓ v9.6 |
| 6 | Cross-reference uploaded spreadsheet, add missing prompts | ✓ v9.6 (Authority Builder Pack: 1,024 new) |
| 7 | In-depth quality review | ✓ v9.6 |
| 8 | Nav bar filter optimisation + more filters | ✓ **v9.7** (Channel Type in v9.6 + tile filters in v9.7) |
| 9 | Save favorite preset selections | ✓ v9.6 |
| 10 | Export functions optimised | ✓ **v9.7** (verified + 2 bugs fixed) |
| 11 | Categories: split platforms vs niches | ✓ v9.6 |

---

## v9.6 Database + v8.9.2 App — Authority Builder Pack + Terminology Cleanup — 2026-05-02

Added 1,024 new prompts (8 stock categories × 128 niches): Hero Images, About Page, Blog Headers, Social Backgrounds, Lifestyle Shots, Product Mockups, Workspaces, Celebration. Each threads through niche-specific environment + props from v9.5 templates while maintaining the cinematic voice from the uploaded Authority Builder Prompt Pack spreadsheet.

Other v9.6 fixes: Recommended Dimensions normalized from 8+ messy formats to 20 canonical forms. Added Channel Type column splitting Profession (3,519) from Platform (900). Hex/RGB color input in wizard with two-way picker binding. Tightened modal paddings ~40px. Saved presets in modal with localStorage persistence. Channel Type nav filter.

## v9.5 — 2026-05-02

22 niche template fixes (Fitness no longer in kitchen, Sleep no longer in wellness kitchen, Photographers no longer in artist studio, etc). Real photo analysis replacing the one-pixel sampler. 15-region geo-context system. Toast z-index fix.

## v9.4 — 2026-05-02

164 platform prompts to fill coverage gaps. v94 modal UX wired.

## v9.3 — 2026-05-02

Database design system: Background, Design Principle, Render Style, Conversion Intent, Recommended Dimensions, Prompt Recipe.

## v9.2 — 2026-05-02

Niche-specific summaries (156 → 2,053 unique). 132 niche profiles. 160 asset types. User-friendly titles.

## Earlier

v9.1, v9.0 — see prior changelogs.
