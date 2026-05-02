# The Prompt Studio v8.9.8 (App) + v9.9 (Database)

**4,437 prompts × 0 duplicate names × 3,202-char average master-context brief per prompt × 7 export formats with full prompt psychology × resolution dropdown × HEX/RGB palette toggle × pixel-size tile badges × prompt psychology marked AUTHORITATIVE everywhere.**

**Repository:** https://github.com/authoritybuilder/ThePromptStudio
**Live deployment:** `https://authoritybuilder.github.io/ThePromptStudio/`
**Owner:** Yasmin Cronin · Authority Builder™
**Community:** https://www.skool.com/authority-builder-9958/about

---

## What's new in v9.9

The previous exports were "surface-level shit" — they only carried the rendered prompt + brand block. The whole point of the database is the per-prompt depth: Industry Context, Cinematic Scenario, Lighting Specification, Wardrobe, Framing, Pose Direction, Design Principle, Conversion Intent, Archetype Blend, Recipe. None of that was reaching the exports or the runtime augmenter.

v9.9 builds a `Rich Brief` column on every one of the 4,437 prompts (avg 3,202 chars, range 1,720-4,938) that concatenates all relevant master columns into a structured block, then threads it through:

- **Runtime augmenter** (`v94AugmentPrompt`): when a tile is clicked, the full per-prompt JSON is fetched and the rich brief is injected into the prompt as `=== ENRICHED CONTEXT (from prompt master) ===` with explicit `AUTHORITATIVE` framing for Design Principle and Conversion Intent.
- **All 7 export formats** (md, skill, claude-project, chatgpt-gpt, json, notion, canva-brief): every download now carries the Master Context block.

### The 9 brief items

| # | Item | Status |
|---|---|---|
| 1 | Export enrichment | ✓ Rich Brief threaded through 7 formats |
| 2 | Palette HEX/RGB toggle | ✓ 3-state toggle (off/HEX/RGB), default OFF |
| 3 | Unique tile names from spreadsheet | ✓ 1,334 dupes → 0 dupes |
| 4 | Pixel-size badge on tiles | ✓ green pill next to aspect badge |
| 5 | "Recommended sizes" panel removed | ✓ disabled |
| 6 | Resolution dropdown | ✓ 25+ options, wired to `state.size` |
| 7 | Prompt master concatenation | ✓ richBrief into runtime + exports |
| 8 | Prompt psychology applied | ✓ AUTHORITATIVE language for Design Principle + Conversion Intent |
| 9 | Enhance | ✓ avg 3,202 chars master context per prompt |

---

## Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.8 — 9-item brief addressed | ~651 KB |
| `index.json` | v9.9 catalogue (4,437 unique-name scenes) | ~5.4 MB |
| `PROMPTSTUDIO-rebuilt.zip` | 4,437 prompt JSONs each with `richBrief` | ~14.9 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.9 — 24 sheets, 4,437 × 46 cols, Rich Brief column | ~4.3 MB |

---

## Deploy

```bash
cd ThePromptStudio
cp /path/to/v9.9/* .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.8 + v9.9 — 9-item brief"
git push origin main
```

Then **hard-refresh:**
- Laptop: Cmd+Shift+R (Mac) or Ctrl+Shift+F5 (PC)
- iPhone: Settings → Safari → Clear History and Website Data

**Important:** From your screenshots, the deployed site appears to be running a hybrid of v9.8 (correct prompt count) and earlier code (chip grids in modal). After pushing v9.9, if you still see anything mismatched, check `https://github.com/authoritybuilder/ThePromptStudio/actions` for failed Pages deployments.

---

## Test after deploy — 9-item checklist

1. **Open any prompt → Export → Download Markdown.** Open the file, scroll past Brand Override Block. You should see a `## Master Context (V99)` section with a code block of structured directives, plus 7 individual subsections (Industry Context, Lighting, Wardrobe, etc).
2. **Click the HEX button** in the Palette section of the brand bar. Hex codes appear under each swatch. Click RGB → switches to comma-separated RGB. Click `·` → labels disappear.
3. **Browse the Tiktok niche.** All 30 tile titles should be visibly different (e.g. "Tiktok Hook Frame for Tiktok", "Tiktok Pattern Break for Tiktok", "Tiktok Reaction Frame for Tiktok"). Italics line below each title is a unique snippet from the cinematic scenario.
4. **Look at any tile.** Two badges in the top-right of the dark preview area: amber `4:5` aspect badge and green `1080x1350` pixel badge.
5. **Browse a niche.** No "Recommended sizes for [platform]" panel between destination chips and tile grid.
6. **Open any prompt.** Modal output banner shows "Change your resolution" with a dropdown (not "auto"). Pick "LinkedIn banner 1584x396" → toast confirms, prompt re-renders with `USER RESOLUTION OVERRIDE: ...` directive.
7. **Open any prompt** and look at the rendered prompt text. Should contain `=== ENRICHED CONTEXT (from prompt master) ===` block before the Brand Override.
8. Inside that ENRICHED CONTEXT block, you should see `DESIGN PRINCIPLE:` and `CONVERSION INTENT:` lines, and below the closing tag, the directive: `The Design Principle and Conversion Intent are AUTHORITATIVE - they describe the psychological response the image must trigger in the viewer.`
9. **Compare prompt length** — the displayed prompt should be substantially longer (typically 4,000-6,000 chars vs 1,500 before) because of the threaded master context.

---

## Owner / Community

- **Owner:** Yasmin Cronin
- **Community:** [Authority Builder on Skool](https://www.skool.com/authority-builder-9958/about)
