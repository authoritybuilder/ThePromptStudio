# CHANGELOG — The Prompt Studio

## v9.8.1 — 6-item screenshot brief — 2026-05-02

Note: items 1, 3, and 6 from this brief were ALREADY shipped in v9.8 — your screenshots show the deployed older version (still on "3,231 prompts" and chip-grid modal). Make sure to deploy v9.8.1 (or v9.8) to see them. Items 2, 4, 5 are new fixes in v9.8.1.

### 1. Niches sidebar scrollbar — already shipped in v9.8

The Niches list section now uses `max-height: 50vh; overflow-y: auto` with a custom 6px scrollbar styled in the brand accent. All 151 niches reachable without scrolling the whole page. **You are seeing this issue because you're viewing the deployed older version.** Push v9.8.1 to fix.

### 2. Tile text from spreadsheet — fixed properly in v9.8.1

The tile renderer was reading `scene.magneticName` first and falling back to `scene.name`. With the v9.8 friendly-name migration, `scene.name` IS the friendly Display Name from the spreadsheet, so the priority was wrong.

Also, "What you get:" was using `v5BuildMagneticSummary()` which synthesised filler text. The user wants it to read directly from the spreadsheet's Summary column.

Fixed in v9.8.1:
- Tile name = `scene.name` (which is the Prompt Title / Display Name from spreadsheet)
- Tagline = `scene.tagline` (newly added — first sentence of the Scenario column)
- "What you get:" = `scene.sub` (which is the Summary column from spreadsheet)
- Falls back to legacy synth only if spreadsheet field is empty

`index.json` regenerated with `tagline` field on all 4,437 scenes. Sample output:

```
name:    "First Call Booking Page Hero for Academic Coaches"
tagline: "Subject mid-discovery-call at desk, leaning slightly forward..."
sub:     "The photo on your booking page that makes prospects feel safe..."
```

### 3. Prompt count — already shipped in v9.8

15 occurrences of "4,437", 0 of "3,231". You're seeing 3,231 because you're viewing the older deployed version.

### 4. Filter bar relocated — collapsible in v9.8.1

The Aspect / Background / Render filter bar that took up significant vertical space at the top of the home page is now **collapsed by default**. Replaced with a single compact "▾ Filters" toggle button (~120px wide, fits inline above the tile grid).

- Click to expand/collapse
- An active-filter count badge appears on the toggle when filters are applied (e.g. "▾ Filters [2]")
- The toggle goes accent-coloured when filters are active, so users know they have filters on even when collapsed
- All chip behaviour and live count badges from v9.7.1 still work — just hidden behind the toggle until needed

The home page now reads: destination chips → tile filter toggle → tile grid. Saves roughly 120-180px of vertical space at the top of every category view on desktop, much more on mobile.

### 5. Design requests field — added in v9.8.1

New textarea in the modal's "Customise this prompt" section, directly below "Specific text or words to include":

> **Do you have any other design requests?**
> _Anything else you want in the image? Mood, props, framing, colour shifts, references — we add it to the prompt._
>
> placeholder: "e.g. 'Add a soft glow around the subject' or 'Make it feel more cinematic' or 'Include a copy of my book on the desk'"

The value gets stored in `window._modalDesignRequests` and threaded into the augmented prompt as:

> ADDITIONAL DESIGN REQUESTS: {user text} - incorporate these creative requests into the scene while maintaining all brand and quality requirements above.

Resets to empty when a new modal opens. Re-renders the prompt display live as you type (so users can see their request appear in the prompt text immediately).

### 6. BG/Render dropdowns — already shipped in v9.8

Both `<select>` elements present, no `v94-bg-chip` or `v94-render-chip` remnants. The screenshot shows the chip grid because you're viewing the older deployed version.

### Files

| File | Status | Size |
|---|---|---|
| `index.html` | App v8.9.7 — 6-item screenshot brief | ~635 KB |
| `index.json` | v9.8.1 catalogue (4,437 scenes, with tagline field) | ~3.8 MB |
| `PROMPTSTUDIO-rebuilt.zip` | v9.8 prompt JSONs (unchanged) | 14.5 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.8 (unchanged) | 4.0 MB |

### Deploy — important note

Your screenshots show that the live site at `authoritybuilder.github.io/ThePromptStudio` is running an older build. To see all 6 items addressed, deploy this v9.8.1 build:

```bash
cd ThePromptStudio
cp /path/to/v9.8.1/index.html .
cp /path/to/v9.8.1/index.json .
cp /path/to/v9.8.1/PROMPTSTUDIO-rebuilt.zip .
cp /path/to/v9.8.1/PromptStudioPro-v9-database.xlsx .
cp /path/to/v9.8.1/CHANGELOG.md .
cp /path/to/v9.8.1/README.md .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.7 + v9.8.1 — 6-item screenshot brief"
git push origin main
```

Then **hard-refresh** your browser:
- **Laptop:** Cmd+Shift+R (Mac) or Ctrl+Shift+F5 (PC)
- **iPhone:** Settings → Safari → Clear History and Website Data, then revisit

If you still see "3,231" or chip grids after a hard-refresh, check `https://github.com/authoritybuilder/ThePromptStudio/actions` for any failed Pages deployment.

### Verification (all 6 items)

- ✓ NICHES SCROLL — `max-height: 50vh` + `overflow-y: auto` in CSS
- ✓ TILE TEXT — name=scene.name, tagline=scene.tagline, summary=scene.sub
- ✓ PROMPT COUNT — 15 instances of "4,437", 0 of "3,231"
- ✓ FILTER BAR — collapsed by default with active-count badge
- ✓ DESIGN REQUESTS — field, state var, prompt threading all wired
- ✓ DROPDOWNS — `<select id="v94BgSelect">` and `<select id="v94RenderSelect">` present, no chip remnants

---

## v9.8 — 11-item brief — 2026-05-02

Prompt count update (3,231 → 4,437). Niches sidebar scroll. Nail Salons niche (18 prompts). YouTube reclassified to Creator Economy. Cross-filter inclusivity (Best Platforms expanded). Em dashes removed (374). Palette hex codes on home page. Friendly tile titles. Readability simplified. Gender field on wizard. BG/Render as dropdowns.

## v9.7.2 — 2026-05-02

New repo (ThePromptStudio), business type as dropdown, fast photo upload.

## v9.7.1 — 2026-05-02

Tile filter chips data-driven with count badges. Regional context in all 8 export formats.

## v9.7 — 2026-05-02

Unified render style taxonomy. Tile-level filter chips. Export pipeline fixes.

## v9.6 — 2026-05-02

Authority Builder Pack (1,024 prompts). Channel Type column. Hex/RGB color input. Saved presets.

## Earlier

v9.5, v9.4, v9.3, v9.2, v9.1, v9.0 — see prior changelogs.
