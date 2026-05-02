# The Prompt Studio v8.9.6 (App) + v9.8 (Database)

**4,437 elite AI image prompts × 151 niches × 8 stock-image categories × 29 unified render styles × 15 regions × hex/RGB colour input with hex labels × saved presets × data-driven tile filters × clean platform/profession split × cross-filter inclusivity × friendly user-facing titles × gender presentation field × compact modal dropdowns × verified export pipeline with regional context in all 8 formats.**

**Repository:** https://github.com/authoritybuilder/ThePromptStudio
**Live deployment** (after GitHub Pages enabled): `https://authoritybuilder.github.io/ThePromptStudio/`
**Owner:** Yasmin Cronin · Authority Builder™
**Community:** https://www.skool.com/authority-builder-9958/about

---

## What's new in v9.8 (11-item brief)

1. **Prompt count corrected** — header and search now show "4,437 prompts" (was "3,231")
2. **Niches sidebar scrolls** — `max-height: 50vh` with custom scrollbar, all 151 niches reachable
3. **Nail Salons niche** — 18 new prompts (8 stock + 10 niche-specific)
4. **YouTube reclassified** — 70 rows moved from Industry "Youtube/Youtubers" to "Creator Economy"
5. **Cross-filter inclusivity** — Breakthrough Coaches × Twitter went from 0 → 25 prompts; same fix across all profession × platform combinations
6. **Em dashes removed** — 374 em dashes replaced with spaced hyphens
7. **Palette hex codes** — each brand-bar swatch now shows its hex code, updates live
8. **Friendly prompt titles** — fixed swapped Display Name / Prompt Title columns; index.json `name` now always reads like "Strategy Session Sales Page Hero for Academic Coaches"
9. **Readability** — simplified 11 high-traffic UI strings to ~grade 9
10. **Gender field** — new wizard field with polite framing, flows into `GENDER PRESENTATION` prompt line
11. **BG/Render as dropdowns** — modal dropdowns with optgroups by family instead of chip grids; the prompt text now sits much higher in the modal

---

## Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.6 — all 11 items | 633 KB |
| `index.json` | v9.8 catalogue (4,437 scenes, friendly names) | 3.4 MB |
| `PROMPTSTUDIO-rebuilt.zip` | All 4,437 prompt JSONs | 14.5 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.8 — 24 sheets, 4,437 × 45 cols | 4.0 MB |

---

## Deploy

```bash
cd ThePromptStudio
cp /path/to/v9.8/index.html .
cp /path/to/v9.8/index.json .
cp /path/to/v9.8/PROMPTSTUDIO-rebuilt.zip .
cp /path/to/v9.8/PromptStudioPro-v9-database.xlsx .
cp /path/to/v9.8/README.md .
cp /path/to/v9.8/CHANGELOG.md .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.6 + v9.8 — 11-item targeted patch"
git push origin main
```

If GitHub Pages isn't enabled yet: Settings → Pages → Source: `main` branch → root folder.

Hard-refresh after deploy: Cmd+Shift+R (laptop) or clear Safari data (iPhone).

---

## Test after deploy

1. **Prompt count** — header reads "4,437 Professional Prompts"
2. **Niches sidebar** — long list of niches scrolls inside the section, doesn't push the page
3. **Nail Salons niche** — appears in sidebar under Profession; click → 18 prompts
4. **YouTube** — Industry filter no longer lists "Youtube" or "Youtubers"; Channel Type → Platform still shows YouTube
5. **Cross-filter** — search for or browse Breakthrough Coaches, then platform-filter to Twitter/X — prompts appear
6. **No em dashes** — scan any wizard hint, modal label, button label
7. **Palette hex codes** — wizard step 2, then look at brand-bar palette: hex codes visible under each swatch
8. **Friendly titles** — open any prompt; the title says "Hero Image for [Niche]" not "Quote-Card Pull Visual"
9. **Readability** — wizard subtitle reads "Set up your prompts in 60 seconds"
10. **Gender field** — wizard Step 1 has new field 3a "How should images of you be rendered?"
11. **Dropdowns** — open any prompt, modal shows Background and Render Style as compact `<select>` elements; the prompt text is visible above the fold

---

## All 11 items from this brief — complete

| # | Item | Status |
|---|---|---|
| 1 | Prompt count display update | ✓ |
| 2 | Niches sidebar scroll bar | ✓ |
| 3 | Nail Salons niche | ✓ |
| 4 | YouTube as Industry → Creator Economy | ✓ |
| 5 | Cross-filter empty state fixed | ✓ |
| 6 | Em dashes removed | ✓ |
| 7 | Palette home page hex codes | ✓ |
| 8 | Friendly prompt titles | ✓ |
| 9 | Readability score 9 | ✓ |
| 10 | Gender field on wizard | ✓ |
| 11 | BG/Render Style as dropdowns | ✓ |

---

## Owner / Community

- **Owner:** Yasmin Cronin
- **Community:** [Authority Builder on Skool](https://www.skool.com/authority-builder-9958/about)
