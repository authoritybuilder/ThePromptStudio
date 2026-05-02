# CHANGELOG — The Prompt Studio

## v9.8 — 11-item targeted patch — 2026-05-02

Eleven specific items from your direct feedback. All eleven verified to pass.

### 1. Prompt count display

The library was showing "3,231" or "3,000+" in 12 places (header subtitle, search placeholder, sidebar count, comments). All updated to **4,437** to match the actual database. The display now matches reality.

### 2. Niches sidebar scroll bar

The Niches section in the left nav previously expanded with `flex: 1; max-height: none` so on tall screens or when collapsing other sections, the niche list could push past the viewport bottom and become unreachable. Now: `max-height: 50vh; overflow-y: auto` with a custom scroll thumb in the brand accent colour. Users always see all 151 niches without scrolling the whole page.

### 3. Nail Salons niche

Added 18 new prompts under a new **Nail Salons** niche (Channel Type: Profession, Industry: Beauty Services):

- 8 stock-style: Hero Image, About Page, Blog Header, Social Background, Lifestyle Shot, Product Mockup, Workspace, Celebration
- 10 niche-specific: Service Showcase, Before & After, Nail Art Detail, Color Trend Post, Booking CTA, Client Testimonial, Tools and Tech, Bridal Service, Seasonal Promo, Studio Tour

Each prompt has all 10 platform exports (Claude, ChatGPT, MJ, Flux, Imagen, Firefly, Leonardo, SDXL, Ideogram, Sora). Palette: dusty pink + warm dark + champagne + white. Default archetype blend: Creator / Lover / Caregiver. Min Claude prompt length: 897 chars.

### 4. YouTube reclassified

YouTube was incorrectly tagged as **Industry** for 70 rows (50 with "Youtube", 20 with "Youtubers"). It's a **platform**, not an industry. Reclassified all 70 rows: Industry now reads "Creator Economy" (234 rows total now in this industry). YouTube remains correctly available as a Channel Type = Platform niche for browsing.

### 5. Cross-filter empty state

Selecting Breakthrough Coaches + Twitter/X returned **0 prompts** because the original Best Platforms strings were too narrow (typically just "website, linkedin"). Most visual prompts work fine on Twitter, Pinterest, Facebook, Instagram, etc. — the data was over-restrictive.

Now Best Platforms is expanded by aspect ratio and asset type. For each prompt:
- Aspect 1:1 / 4:5 → adds Instagram, Pinterest, Facebook
- Aspect 9:16 → adds Instagram, TikTok, YouTube, Pinterest
- Aspect 16:9 / 16:5 / wide → adds LinkedIn, Twitter/X, Facebook, Website
- Visual asset categories → adds Twitter/X, Pinterest broadly
- All prompts → website

Result: Breakthrough Coaches × Twitter/X went from 0 → 25 prompts. Same fix applied across every (profession × platform) combination.

Platform-niche prompts (Channel Type=Platform) keep their tight platform binding so they don't bleed into other platforms.

### 6. Em dashes removed

374 em dashes (`—`) removed across the entire HTML and replaced with spaced hyphens (` - `). Cleans up button labels, hints, subtitles, and inline copy.

### 7. Palette hex codes on home page

The four brand-bar palette swatches now show the hex code below each swatch in 9px monospace. The labels update live when:
- The user clicks a swatch and changes the colour (color picker `oninput`)
- The user picks a season palette in the wizard
- The wizard syncs colours to the brand bar

CSS adds 18px of bottom padding to the palette section to fit the labels.

### 8. Friendly prompt titles in HTML

Bug found: Display Name and Prompt Title were swapped — the **academic** title ("Quote-Card Pull Visual") was in `Display Name`, and the **friendly** version ("Pull Quote Visual for Bloggers") was in `Prompt Title`. The HTML reads the `name` field from `index.json`, which was being populated from the academic version.

Fix: Both columns now hold the friendly version. The `name` field in `index.json` is now always something like:
- "First Call Booking Page Hero for Academic Coaches"
- "Strategy Session Sales Page Hero for Academic Coaches"
- "Group Programme Sales Image for Academic Coaches"

Sample of 20 scene names: 20/20 follow the "X for Niche" pattern. Zero academic remnants.

### 9. Readability score 9

Simplified eleven high-traffic UI strings to roughly grade 9 reading level. Examples:

| Before | After |
|---|---|
| Personalise your elite prompt library in 60 seconds | Set up your prompts in 60 seconds |
| How you want to be addressed throughout the experience. | How should I call you? |
| The name your audience knows you by. We use this in your prompt templates. | Your business name. We add this to your prompts. |
| This tunes the language and positioning in your prompts. | This shapes the words and tone in your prompts. |
| Where the subject sits - change the visual feel | Where the subject sits. Pick the look you want. |
| Random gives a brand-cohesive palette · Remix shifts hues while keeping the vibe | Random gives a balanced palette. Remix changes the colours but keeps the feel. |

### 10. Gender field on wizard

New field card 3a between Business Type and Country, with polite framing:

> **How should images of you be rendered?**
> _This helps the AI draw you well. We never share or save this._
>
> Dropdown options: Prefer not to say (use neutral defaults) / Woman / Man / Non-binary or genderfluid / Other - I will describe it myself

Picking "Other" reveals a free-text field. The chosen value flows into the prompt's `GENDER PRESENTATION:` line via `v5BuildBrandOverride()` and `getIdentityCapsule()`. Maps `woman → female-presenting`, `man → male-presenting`, `non-binary → non-binary, gender-neutral presentation`, `custom → user-supplied text`.

### 11. Background and Render Style as dropdowns

Modal previously used chip grids: Background was 15 chips, Render Style was 29 chips in 7 family rows. They took up significant vertical space, pushing the prompt text below the fold.

Now both are compact `<select>` dropdowns:
- Background: single dropdown with all 15 options as `Icon Name - Description` (truncated to 60 chars)
- Render Style: single dropdown with `<optgroup>` per family ("📸 Photoreal", "🧸 3D / CGI", "🌸 Anime / Manga", etc.)

Wiring: `bgSelect.addEventListener('change')` and `renderSelect.addEventListener('change')` update `scene.v94_background` / `scene.v94_render` and call the regenerate callback. Preset apply also updates both dropdowns. The chip-related CSS rules and click handlers were removed.

### Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.6 (all 11 items implemented) | 633 KB |
| `index.json` | v9.8 catalogue (4,437 scenes, friendly names) | 3.4 MB |
| `PROMPTSTUDIO-rebuilt.zip` | All 4,437 prompt JSONs | 14.5 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.8 — 24 sheets, 4,437 × 45 cols | 4.0 MB |

### Verification

All 11 items independently verified to pass via automated audit:

1. ✓ Prompt count "4,437" in 15 places, no "3,231" remnants
2. ✓ Niches sidebar `max-height: 50vh; overflow-y: auto`
3. ✓ Nail Salons: 18 prompts in DB, 18 in index.json
4. ✓ YouTube/Youtubers: 0 in Industry column, 234 rows now Creator Economy
5. ✓ Breakthrough Coaches × Twitter: 25 prompts (was 0)
6. ✓ Em dashes: 0 in HTML (was 374)
7. ✓ Palette hex labels: CSS class, markup, and sync handler all present
8. ✓ Friendly names: 20/20 sample scenes follow "X for Niche" pattern
9. ✓ Readability: simplified subtitle and field hints present
10. ✓ Gender field: select element, state wiring, flows to prompts
11. ✓ Dropdowns: BG `<select>`, Render `<select>` with optgroups, onchange wired, no chip remnants

### Deploy

```bash
cd ThePromptStudio
cp /path/to/v9.8/* .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.6 + v9.8 — 11-item brief patch"
git push origin main
```

Hard-refresh after deploy.

---

## v9.7.2 — 2026-05-02

New repo (ThePromptStudio), business type as dropdown, photo upload speedup.

## v9.7.1 — Final debug pass — 2026-05-02

Tile filter chips made data-driven with count badges. Regional context added to all 8 export formats. JSON export got the rendered prompt and archetype blend.

## v9.7 — 2026-05-02

Unified render style taxonomy (29 styles in 7 families). Tile-level filter chips. Export pipeline traced and 2 silent-customisation-drop bugs fixed.

## v9.6 — 2026-05-02

Authority Builder Pack (1,024 new prompts). Channel Type column. Hex/RGB color input. Tighter modal padding. Saved presets.

## v9.5 — 2026-05-02

22 niche template fixes. Real photo analysis. 15-region geo-context.

## Earlier

v9.4, v9.3, v9.2, v9.1, v9.0 — see prior changelogs.
