# CHANGELOG — The Prompt Studio

## v9.9.1 — 3-bug hotfix — 2026-05-02

I broke three things in v9.9. This release fixes them. Honest accounting:

### Bug 1: Tiles weren't showing under niches (regression I introduced in v9.9)

**Root cause:** When I rebuilt index.json for v9.9, I wrote `sceneIds` (camelCase) on every category. The entire HTML codebase reads `scene_ids` (snake_case) — there are 8+ places in index.html that do `cat.scene_ids` or `cat.scene_ids.indexOf(...)`. So the loop that renders tiles found `undefined` and silently rendered nothing.

**Fix:** Categories are now built with `scene_ids` (snake_case) plus a `scene_count` helper. Verified in audit — sample category has `scene_ids` array of 23 IDs that match real prompts.

This is the kind of bug that's purely my fault — I should have grep-checked the field name against the existing app code before changing it. Sorry for the wasted refresh cycles.

### Bug 2: Tile names not pulling from columns B/C/D as requested

**What you asked for:**
- Tile bold title = column B "Display Name"
- Italic sub-text = column C "Prompt Title"
- Description ("What you get:") = column D "Summary"

**What was wrong:** In v9.8 I synced columns B and C to be identical (both held the friendly "X for Niche" version). So even though the HTML mapping was correct, columns B and C had the same text — the tiles rendered the same string twice.

**Fix:** Column C "Prompt Title" is now derived from `Asset Category` + `Lighting Variant` to produce a craft-level descriptor that's visibly different from column B. Examples from the Tiktok niche:

| ID | name (col B, bold) | tagline (col C, italic) | sub (col D, "What you get:") |
|---|---|---|---|
| tiktok-001 | Tiktok Video Hook Frame for Tiktok | Video · Hook | The first-three-seconds attention-grabbing frame… |
| tiktok-002 | Tiktok Pattern Break for Tiktok | Advertising · Disruption | The scroll-stopping disruption image for ad creative… |
| tiktok-003 | Tiktok Call to Action Card for Tiktok | Conversion · CTA | The action-driving image with clear CTA… |
| tiktok-004 | Tiktok Video Cover Thumbnail for Tiktok | Video · Thumbnail | The click-worthy cover for video content… |
| tiktok-005 | Tiktok Editorial Quote Pull for Tiktok | Editorial · Premium | The magazine-style quote pull for premium editorial… |
| tiktok-006 | Tiktok Reaction Frame for Tiktok | Video · Reaction | The expression-driven thumbnail for reaction or commentary… |

Three visibly distinct fields, all sourced from the spreadsheet, all unique within the niche. Per-prompt JSONs and the workbook's "Prompts MASTER" sheet also updated so columns B and C carry these distinct values.

### Bug 3: "Skip for now" on wizard wasn't working

**Root cause:** `completeWizard()` was reading wizard fields without null-guards. A single missing element or read error would throw an exception and halt execution before the line that hides the wizard. Result: button click → silent JS error → wizard stays on screen.

**Fix:** Wrapped every field read in try/catch, with `console.warn` logging if a field is missing. The two critical lines that close the wizard (`wizard.style.display = 'none'` and `app.style.display = 'block'`) now run unconditionally — they cannot be blocked by a field-read failure earlier in the function. Any sub-component init failures (brand bar sync, auto-load, auto-save) also wrapped individually so one failure can't stop another from running.

### Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.8.1 — 3-bug hotfix | ~652 KB |
| `index.json` | v9.9.1 — scene_ids fix + distinct B/C/D | ~5.7 MB |
| `PROMPTSTUDIO-rebuilt.zip` | All 4,437 prompt JSONs with distinct B/C/D + richBrief | ~15.0 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.9 workbook (unchanged from v9.9) | ~4.3 MB |

### Verification

4/4 automated checks pass:

- ✓ Categories have `scene_ids` (snake_case) — tiles will render
- ✓ Tiktok niche shows 6 distinct B/C/D combinations
- ✓ Wizard `completeWizard()` wrapped in try/catch
- ✓ Tile renderer reads `scene.name` / `scene.tagline` / `scene.sub` (cols B / C / D)

### Deploy

```bash
cd ThePromptStudio
cp /path/to/v9.9.1/index.html .
cp /path/to/v9.9.1/index.json .
cp /path/to/v9.9.1/PROMPTSTUDIO-rebuilt.zip .
cp /path/to/v9.9.1/CHANGELOG.md .
cp /path/to/v9.9.1/README.md .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.8.1 + v9.9.1 — hotfix: tile loading, B/C/D mapping, wizard skip"
git push origin main
```

Hard-refresh after deploy.

### What's still in v9.9 (unchanged, all working)

- 4,437 prompts × 0 duplicate names within niches
- Rich Brief column with 1,720-4,938 char per-prompt master context
- Master Context section in all 7 export formats
- HEX/RGB palette toggle
- Pixel-size tile badges
- Resolution dropdown in modal
- ENRICHED CONTEXT block threaded into runtime prompts
- Design Principle + Conversion Intent flagged AUTHORITATIVE everywhere
- Recommended Sizes panel removed

---

## v9.9 — 9-item screenshot brief — 2026-05-02

Export enrichment via Rich Brief column. HEX/RGB palette toggle. Unique tile names (1,334 dupes → 0). Pixel-size badges. Recommended Sizes panel removed. Resolution dropdown. Prompt context concatenation. Prompt psychology AUTHORITATIVE. Master Context in all exports.

## v9.8.1 — 6-item screenshot brief — 2026-05-02

Tile text from spreadsheet. Filter bar collapsible. Design requests field.

## v9.8 — 11-item brief — 2026-05-02

Prompt count → 4,437. Niches scroll. Nail Salons. YouTube to Creator Economy. Cross-filter inclusivity. Em dashes removed. Friendly titles. Gender field. BG/Render dropdowns.

## Earlier

v9.7.2, v9.7.1, v9.7, v9.6, v9.5, v9.4 etc. — see prior changelogs.
