# The Prompt Studio v8.9.3 (App) + v9.7 (Database)

**4,419 elite AI image prompts × 132 niches × 8 stock-image categories × 29 unified render styles × 15 regions × hex/RGB colour input × saved presets × tile-level filters × clean platform/profession split × verified export pipeline.**

**Live deployment:** https://github.com/authoritybuilder/PromptStudioPro
**Owner:** Yasmin Cronin · Authority Builder™
**Community:** https://www.skool.com/authority-builder-9958/about

---

## What's new in v9.7

### 🎨 Item 3 — Unified render style taxonomy

Wizard had 20 cartoon-style IDs in kebab-case. Modal had 12 different IDs in snake_case. They never synced. Now there's **one** taxonomy of 29 styles in 7 families used everywhere.

Pick "Pixar" in the wizard → open any prompt → Pixar is the active render chip. Modal shows styles grouped under section labels: "📸 Photoreal", "🧸 3D / CGI", "🌸 Anime / Manga", "✏️ Illustration", "🎨 Painted", "💥 Stylised", "🏰 2D Animation".

Backward-compatible: existing prompts with old keys (`photoreal`, `cartoon_modern`, `anime_manga`) still work via the runtime translation layer.

### 🎯 Item 8 — Tile-level filter chips

New horizontal chip bar above the tile grid. Filter prompts by:

- **Aspect** — All / 16:9 / 4:5 / 1:1 / 9:16
- **Background** — All + top 6 styles
- **Render** — All + top 6 unified render styles

Filters compose. Click "Clear" to reset all three. Mobile-responsive.

### 📤 Item 10 — Export pipeline fixed

Found two real bugs in the export path:
1. The brand-bar's "Copy & Open AI" button was silently dropping the user's modal customisations
2. The MD download was doing the same

Both fixed. Modal customisations (background style, render style, platform format override) now flow through to every export that uses a single prompt: modal Copy / Copy & Open, MD download, and brand-bar Copy.

The other 7 export formats (SKILL.md, Claude Project, Custom GPT, JSON, Notion, Skool, Canva Brief) build brand context bundles rather than single-prompt exports — they're correct as-is.

---

## All 11 items from your original brief — complete

1. ✓ Pixel/platform terminology consistency *(v9.6)*
2. ✓ Hex / RGB colour input in wizard *(v9.6)*
3. ✓ Cartoon choices align with popup tiles *(v9.7 — unified taxonomy)*
4. ✓ UI/wizard/tile terminology cross-reference *(v9.6)*
5. ✓ Reduce wasted space in popup tiles *(v9.6)*
6. ✓ Cross-reference uploaded spreadsheet, add missing prompts *(v9.6 — 1,024 new)*
7. ✓ In-depth quality review *(v9.6)*
8. ✓ Nav bar filters optimised *(v9.6 + v9.7 tile filters)*
9. ✓ Save favorite preset selections *(v9.6)*
10. ✓ Export functions optimised *(v9.7 — verified + 2 bugs fixed)*
11. ✓ Categories: platforms split from niches *(v9.6)*

---

## Files in this release

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.3 (unified render styles, tile filters, export fix, all v9.6 features) | 623 KB |
| `index.json` | v9.7 catalogue (4,419 scenes, migrated render keys) | 3.5 MB |
| `PROMPTSTUDIO-rebuilt.zip` | All 4,419 prompt JSONs (render keys migrated) | 15.8 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.7 — 24 sheets, 4,419 × 45 cols | 3.94 MB |
| `README.md` + `CHANGELOG.md` | Updated | — |

---

## Quality stats

- **4,419 prompts × 45 columns**
- **29 unified render styles** in 7 families (was 32 fragmented across UI)
- **3 tile-level filter dimensions** (was 0)
- **5+ export paths** piping through `v94AugmentPrompt` for full customisation flow (was 2)
- Min/median/max Claude prompt length: **1,544 / 1,934 / 2,741 chars**
- Profession prompts: 3,519 / Platform prompts: 900
- 20 canonical Recommended Dimensions formats (was 8+ messy variants)

---

## Deploy

```bash
cd PromptStudioPro
cp /path/to/v9.7/index.html .
cp /path/to/v9.7/index.json .
cp /path/to/v9.7/PROMPTSTUDIO-rebuilt.zip .
cp /path/to/v9.7/PromptStudioPro-v9-database.xlsx .
cp /path/to/v9.7/README.md .
cp /path/to/v9.7/CHANGELOG.md .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.3 + v9.7 — unified render styles, tile filters, export pipeline fix"
git push
```

**Hard-refresh after deploy:**
- Laptop: Cmd+Shift+R / Ctrl+Shift+F5
- iPhone: Settings → Safari → Clear History and Website Data

---

## Test the v9.7 changes

1. **Wizard photo style step** — pick "Cartoon / Illustration" → choose "Pixar" → open any prompt → confirm Pixar chip is active in the modal's Render Style picker
2. **Modal render styles** — open any prompt → scroll to Render Style picker → confirm styles are grouped under family labels (Photoreal / 3D / Anime / etc.)
3. **Tile filters** — on any category page → click "Aspect: 4:5" + "Render: Watercolour" → confirm tiles filter to only matching prompts → click "Clear" to reset
4. **Export pipeline** — open any prompt → in the modal pick a different background and render style → close modal → click the brand-bar Copy button → paste somewhere → confirm the customisations are in the copied text
5. **MD download** — open any prompt → in modal pick custom background → use export bar to download `.md` → open file → confirm `BACKGROUND OVERRIDE:` line is present

---

## Owner / Community

- **Owner:** Yasmin Cronin
- **Community:** [Authority Builder on Skool](https://www.skool.com/authority-builder-9958/about)
