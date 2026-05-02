# The Prompt Studio v8.9.7 (App) + v9.8.1 (Database)

**4,437 elite AI image prompts × 151 niches × tile labels straight from spreadsheet × collapsible filter bar × design-requests field × dropdowns × all v9.8 improvements.**

**Repository:** https://github.com/authoritybuilder/ThePromptStudio
**Live deployment** (after GitHub Pages enabled): `https://authoritybuilder.github.io/ThePromptStudio/`
**Owner:** Yasmin Cronin · Authority Builder™
**Community:** https://www.skool.com/authority-builder-9958/about

---

## Important: deploy v9.8.1 to see the changes

Your screenshots show the live site at `authoritybuilder.github.io/ThePromptStudio` is running an older build (still shows "3,231 prompts", chip grids in modal, etc). All six items from your screenshot brief are addressed in this v9.8.1 build — three of them were already shipped in v9.8 but never deployed, three are new in v9.8.1.

**Deploy steps:**

```bash
cd ThePromptStudio
cp /path/to/v9.8.1/* .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.7 + v9.8.1 — 6-item screenshot brief"
git push origin main
```

Then **hard-refresh:**
- Laptop: Cmd+Shift+R (Mac) or Ctrl+Shift+F5 (PC)
- iPhone: Settings → Safari → Clear History and Website Data, then revisit

---

## What's in v9.8.1

### Items already in v9.8 (deploy to see them)

1. **Niches sidebar scroll bar** — `max-height: 50vh` with custom scrollbar. All 151 niches reachable.
3. **Prompt count corrected** — header now reads "4,437 Professional Prompts" (15 instances replaced).
6. **Background/Render as dropdowns** — both modal pickers are `<select>` elements with optgroups by family. Chip grids removed entirely.

### New in v9.8.1

2. **Tile text reads from spreadsheet directly**
   - Tile name = `scene.name` (= Display Name / Prompt Title from spreadsheet)
   - Tagline = `scene.tagline` (= first sentence of Scenario, newly added to all 4,437 scenes)
   - "What you get:" = `scene.sub` (= Summary column from spreadsheet)
   - Falls back to legacy synth only if a spreadsheet field is empty

4. **Filter bar collapsible** — the Aspect / Background / Render filter chip rows that took up ~120-180px at the top of every category view are now collapsed behind a single "▾ Filters" toggle. Click to expand. An active-filter count badge appears on the toggle when filters are applied. The toggle goes accent-coloured when filters are active.

5. **Design requests field** — new textarea in the modal "Customise this prompt" section, directly below "Specific text or words to include". Placeholder: "e.g. 'Add a soft glow around the subject' or 'Make it feel more cinematic' or 'Include a copy of my book on the desk'". Threaded into the prompt as `ADDITIONAL DESIGN REQUESTS: ... - incorporate these creative requests into the scene while maintaining all brand and quality requirements above.`

---

## Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.7 — 6-item brief addressed | ~635 KB |
| `index.json` | v9.8.1 (4,437 scenes, with tagline field) | ~3.8 MB |
| `PROMPTSTUDIO-rebuilt.zip` | v9.8 prompt JSONs (unchanged) | 14.5 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.8 (unchanged) | 4.0 MB |
| `CHANGELOG.md` + `README.md` | Updated | — |

---

## Test after deploy

1. **Niches sidebar** — long list of niches scrolls inside the section, doesn't push the whole page
2. **Tile text** — pick any niche, look at any tile: bold name should read "X for [Niche]", italic tagline should be a snippet of the cinematic scene description, "What you get:" should be a benefit-focused sentence (not generic filler)
3. **Prompt count** — header subtitle reads "4,437 Professional Prompts"
4. **Filter bar** — Aspect/Background/Render chips no longer dominate the home page; a small "▾ Filters" pill sits above the tile grid; click it to expand
5. **Design requests** — open any prompt → modal → scroll to "Customise this prompt" section → see the new textarea below "Specific text or words". Type something → confirm it appears in the prompt text below
6. **Dropdowns** — open any prompt → see Background Style and Render Style as compact `<select>` dropdowns, not chip grids

---

## Owner / Community

- **Owner:** Yasmin Cronin
- **Community:** [Authority Builder on Skool](https://www.skool.com/authority-builder-9958/about)
