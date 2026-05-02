# CHANGELOG — The Prompt Studio

## v9.7.2 — 2026-05-02 — UX patch

Four targeted fixes from your direct feedback.

### 1. New repository URL

Updated the `GITHUB` constant in `index.html` from `PromptStudioPro` to `ThePromptStudio`. All raw-content fetches (`index.json`, individual prompt JSONs) now point at `https://raw.githubusercontent.com/authoritybuilder/ThePromptStudio/main/...`. README and CHANGELOG also updated.

### 2. Broken GitHub link in docs — fixed

The README pointed at `https://github.com/authoritybuilder/PromptStudioPro` which doesn't exist. The actual repo is at `https://github.com/authoritybuilder/ThePromptStudio.git`. Both files now point at the correct URL.

### 3. Business Type as dropdown (was 17-tile grid)

The wizard's Step 1 had a 17-tile `.bt-grid` taking up significant vertical space. Replaced with a single compact `<select>`. The "Other..." option still reveals a text input below for custom business types. Saved roughly 200px of wizard height on desktop, ~280px on mobile.

The dropdown list (in order):
- Coach, Consultant, Creator, Speaker, Author, Course Creator, Agency Owner, Service Provider, Product Founder, SaaS Founder, Real Estate, Wellness Pro, Fitness Pro, Healer / Energy Worker, Therapist, Attorney / Lawyer, Other...

### 4. Photo upload speed — substantially faster

The previous flow was painful on large photos:
- Used `FileReader.readAsDataURL` which produces a base64 string of the FULL uncompressed image (a 5MB photo = ~6.7MB base64)
- Set the thumbnail's `<img src>` to that full base64 — bloating the DOM
- Ran feature extraction on every photo (only the first is needed for the identity capsule)

New flow:
- Uses `URL.createObjectURL(file)` for instant load (no base64 encoding pass)
- Compresses each thumbnail to **320px on the long edge as JPEG quality 0.78** — typical thumbnail is ~25KB instead of 6MB
- Shows a "Loading…" placeholder in each slot immediately, replaced with the thumbnail when ready
- Calls `URL.revokeObjectURL` after thumbnail generation to free memory
- Runs `v3ExtractPhotoFeatures` ONLY on the first photo (sufficient for the brand identity capsule — additional photos only contribute season-detection votes)
- The toast now confirms what was detected: "✓ Detected: medium skin (warm), rich warm brown, dark brown to deep brown eyes"

Same pipeline applied to inspiration uploads.

**Expected speedup on 10 photos:** roughly 4-6× faster perceived speed (instant slot fill, then thumbnails populating in parallel as compression completes).

### Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.5 — repo URL, business type dropdown, fast photo upload | ~625 KB |
| `index.json` | v9.7 catalogue (4,419 scenes) — unchanged | 3.5 MB |
| `PROMPTSTUDIO-rebuilt.zip` | All 4,419 prompt JSONs — unchanged | 15.8 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.7 — unchanged | 3.94 MB |

### Deploy to the new repo

```bash
# First time setup with the new repo
git clone https://github.com/authoritybuilder/ThePromptStudio.git
cd ThePromptStudio
cp /path/to/v9.7.2/* .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.5 + v9.7.2 — new repo, business type dropdown, fast photo upload"
git push origin main
```

Then enable GitHub Pages: Settings → Pages → Source: `main` branch, root folder.

After GitHub Pages is enabled, the live URL becomes `https://authoritybuilder.github.io/ThePromptStudio/`.

**Hard-refresh after deploy:** Cmd+Shift+R (laptop) or clear Safari data (iPhone).

---

## v9.7.1 — Final debug pass — 2026-05-02

Four real bugs found and fixed by tracing data flow:
1. Tile filter chips were hardcoded too narrow — only 4 of 20 used aspect ratios shown. Now data-driven from visible scenes with live count badges.
2. All 8 export formats were silently dropping the user's region. Now `v97CountryName` and `v97RegionalContext` thread through every format.
3. JSON export was missing the rendered prompt text and archetype blend. Both added.
4. Skool community-share post was missing palette. Added.

## v9.7 — 2026-05-02

Unified render style taxonomy (29 styles in 7 families). Tile-level filter chips. Export pipeline traced and 2 silent-customisation-drop bugs fixed.

## v9.6 — 2026-05-02

Authority Builder Pack (1,024 new prompts: 8 stock categories × 128 niches). Channel Type column. Hex/RGB color input. Tighter modal padding. Saved presets. Recommended Dimensions normalized.

## v9.5 — 2026-05-02

22 niche template fixes. Real photo analysis. 15-region geo-context. Toast z-index fix.

## Earlier

v9.4, v9.3, v9.2, v9.1, v9.0 — see prior changelogs.
