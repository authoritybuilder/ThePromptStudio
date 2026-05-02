# The Prompt Studio v8.9.5 (App) + v9.7 (Database)

**4,419 elite AI image prompts × 132 niches × 8 stock-image categories × 29 unified render styles × 15 regions × hex/RGB colour input × saved presets × data-driven tile filters × clean platform/profession split × verified export pipeline × fast photo upload.**

**Repository:** https://github.com/authoritybuilder/ThePromptStudio
**Live deployment** (after GitHub Pages enabled): `https://authoritybuilder.github.io/ThePromptStudio/`
**Owner:** Yasmin Cronin · Authority Builder™
**Community:** https://www.skool.com/authority-builder-9958/about

---

## What's new in v9.7.2 (UX patch)

- **New repo:** updated all references from `PromptStudioPro` to `ThePromptStudio`
- **Business type as dropdown:** wizard Step 1 shrinks by ~200-280px — single compact `<select>` instead of a 17-tile grid
- **Fast photo upload:** ~4-6× perceived speedup. Switched from `readAsDataURL` (base64 of full image) to `createObjectURL` + canvas-compressed 320px thumbnails. Only the first photo runs feature extraction; subsequent photos just contribute to season-detection voting

---

## What v9.7 brought before this patch

- **29 unified render styles** in 7 families (replacing 32 fragmented IDs across wizard + modal)
- **Data-driven tile filter chips** with live count badges — every used aspect ratio, background, and render is reachable
- **Regional context in all 8 export formats** — Australia/UK/Japan/etc. wizard selection now flows into MD, SKILL.md, JSON, Notion, Skool, Canva Brief, Custom GPT, Claude Project
- **Authority Builder Pack:** 1,024 new prompts (8 stock categories × 128 niches)
- **Saved presets** in the modal with localStorage persistence
- **Hex / RGB color input** in the wizard
- **Channel Type filter** splitting platforms (22) from professions (110)

---

## Files in this release

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.5 — new repo URL, business type dropdown, fast photo upload | ~625 KB |
| `index.json` | v9.7 catalogue (4,419 scenes) | 3.5 MB |
| `PROMPTSTUDIO-rebuilt.zip` | All 4,419 prompt JSONs | 15.8 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.7 — 24 sheets, 4,419 × 45 cols | 3.94 MB |
| `README.md` + `CHANGELOG.md` | Updated for v9.7.2 | — |

---

## Deploy (first time with the new repo)

```bash
git clone https://github.com/authoritybuilder/ThePromptStudio.git
cd ThePromptStudio
cp /path/to/v9.7.2/index.html .
cp /path/to/v9.7.2/index.json .
cp /path/to/v9.7.2/PROMPTSTUDIO-rebuilt.zip .
cp /path/to/v9.7.2/PromptStudioPro-v9-database.xlsx .
cp /path/to/v9.7.2/README.md .
cp /path/to/v9.7.2/CHANGELOG.md .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.5 + v9.7.2 — initial commit on new repo"
git push origin main
```

Then enable GitHub Pages:
1. Go to https://github.com/authoritybuilder/ThePromptStudio/settings/pages
2. Source: Deploy from a branch
3. Branch: `main` / Folder: `/ (root)`
4. Save

The live URL becomes `https://authoritybuilder.github.io/ThePromptStudio/` after a few minutes. Hard-refresh after deploy.

---

## Test after deploy

1. **Wizard Step 1** — confirm Business Type is a single dropdown, not a tile grid. Pick "Other..." → confirm the text input appears below.
2. **Wizard Step 2** — upload 5+ photos at once. Confirm "Loading…" placeholders appear immediately, replaced by thumbnails as compression completes (should be visibly faster than before).
3. **Wizard Step 2** — first photo upload toast should read something like "✓ Detected: medium skin (warm), rich warm brown, dark brown to deep brown eyes".
4. **Tile filter coverage** — open any niche. Confirm each filter row shows count badges and only chips that exist in the data.
5. **Channel Type filter** — sidebar → "Channel Type" → "Platforms / Channels" → confirm 22 platforms shown.
6. **Saved preset** — open a prompt, customise background + render, click "+ Save current", name it. Open a different prompt → apply the preset → modal chips switch.
7. **Regional context in MD export** — Wizard Step 1 → Country → Australia → save wizard → open any prompt → modal export bar → MD download → confirm "Regional Context (V97)" section is present in the file.

---

## All 11 items from the original brief — complete

1. ✓ Pixel/platform terminology consistency *(v9.6)*
2. ✓ Hex / RGB colour input *(v9.6)*
3. ✓ Cartoon choices align with popup tiles *(v9.7)*
4. ✓ UI/wizard/tile terminology cross-reference *(v9.6)*
5. ✓ Reduce wasted space in popup tiles *(v9.6)*
6. ✓ Cross-reference uploaded spreadsheet, add missing prompts *(v9.6 — 1,024 new)*
7. ✓ In-depth quality review *(v9.6 + v9.7.1 final debug pass)*
8. ✓ Nav bar filters optimised *(v9.6 channel type + v9.7.1 data-driven tile filters)*
9. ✓ Save favorite preset selections *(v9.6)*
10. ✓ Export functions optimised *(v9.7 augmentation pipeline + v9.7.1 regional context)*
11. ✓ Categories: platforms split from niches *(v9.6)*

---

## Owner / Community

- **Owner:** Yasmin Cronin
- **Community:** [Authority Builder on Skool](https://www.skool.com/authority-builder-9958/about)
