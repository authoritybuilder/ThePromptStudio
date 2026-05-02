# The Prompt Studio v8.9.8.1 (App) + v9.9.1 (Database)

**Hotfix release.** Three bugs from v9.9 fixed: tile loading, distinct B/C/D mapping, wizard skip button.

**Repository:** https://github.com/authoritybuilder/ThePromptStudio
**Live deployment:** `https://authoritybuilder.github.io/ThePromptStudio/`
**Owner:** Yasmin Cronin · Authority Builder™

---

## What's fixed in v9.9.1

| # | Bug | Cause | Fix |
|---|---|---|---|
| 1 | Tiles not showing under niches | I wrote `sceneIds` (camelCase) but HTML reads `scene_ids` (snake_case) | Categories rebuilt with `scene_ids` — verified |
| 2 | Tile name not pulling from cols B/C/D | Cols B and C were synced to identical values in v9.8 | Col C now derived from Asset Category + Lighting Variant — visibly distinct |
| 3 | "Skip for now" wizard button not working | Field read errors threw before wizard close | Wrapped in try/catch; close lines now run unconditionally |

## What v9.9.1 looks like in the tile

For each Tiktok prompt:
- **Bold tile title** (col B "Display Name"): `Tiktok Video Hook Frame for Tiktok`
- **Italic sub-text** (col C "Prompt Title"): `Video · Hook`
- **"What you get:"** (col D "Summary"): `The first-three-seconds attention-grabbing frame…`

Three visibly distinct fields, all sourced from the spreadsheet, all unique within the niche.

---

## Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.8.1 — 3-bug hotfix | ~652 KB |
| `index.json` | v9.9.1 — scene_ids fix + distinct B/C/D | ~5.7 MB |
| `PROMPTSTUDIO-rebuilt.zip` | 4,437 prompt JSONs with distinct B/C/D + richBrief | ~15.0 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.9 workbook (unchanged) | ~4.3 MB |

---

## Deploy

```bash
cd ThePromptStudio
cp /path/to/v9.9.1/* .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.8.1 + v9.9.1 — hotfix: tile loading, B/C/D mapping, wizard skip"
git push origin main
```

Then **hard-refresh:**
- Laptop: Cmd+Shift+R (Mac) or Ctrl+Shift+F5 (PC)
- iPhone: Settings → Safari → Clear History and Website Data

---

## Test after deploy — 3 critical checks

1. **Click any niche.** Tiles should appear (was: empty list in v9.9).
2. **Look at any tile.** Three distinct text rows: bold title, italic sub-text (e.g. "Video · Hook"), then "What you get:" description.
3. **Open wizard, click "Skip for now".** Wizard closes, app loads.

If all three work, v9.9.1 is good. Then test the v9.9 features (export Master Context, HEX/RGB toggle, pixel-size badges, resolution dropdown, ENRICHED CONTEXT in prompts).

---

## What's still in v9.9 (unchanged, all working)

- 4,437 prompts × 0 duplicate names within niches
- Rich Brief column with 1,720-4,938 char per-prompt master context
- Master Context section in all 7 export formats (md / skill / claude-project / chatgpt-gpt / json / notion / canva-brief)
- HEX/RGB palette toggle (default OFF, click HEX or RGB to reveal)
- Pixel-size badges on tiles
- Resolution dropdown in modal (replaces "auto")
- ENRICHED CONTEXT block threaded into runtime prompts
- Design Principle + Conversion Intent flagged AUTHORITATIVE everywhere
- "Recommended sizes" panel removed
- Filter bar collapsible
- "Do you have any other design requests?" field

---

## Owner / Community

- **Owner:** Yasmin Cronin
- **Community:** [Authority Builder on Skool](https://www.skool.com/authority-builder-9958/about)
