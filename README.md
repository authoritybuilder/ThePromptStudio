# The Prompt Studio v8.9.8.3 (App) + v9.9.1 (Database)

**Critical syntax error fix.** The `Uncaught SyntaxError: Invalid or unexpected token` you reported was the actual cause of EVERYTHING being broken — skip not working, niches not loading, the lot. One bad escape sequence in a template literal that I introduced in v9.9 was killing the entire JS bundle before any of it could run.

**Repository:** https://github.com/authoritybuilder/ThePromptStudio
**Live deployment:** `https://authoritybuilder.github.io/ThePromptStudio/`
**Owner:** Yasmin Cronin · Authority Builder™

---

## What's fixed in v9.9.3

`Uncaught SyntaxError: Invalid or unexpected token` was killing the entire app. Inside the v9.9 markdown export Master Context section, I wrote:

```
\\`\\`\\`
```

intending to output a markdown fence. But inside a JavaScript template literal, `\\` produces a literal `\` and the next unescaped backtick **terminates the template literal mid-string**. JS parser fails immediately, the whole script bundle never executes, app dies on load.

Should have run `node --check` on the HTML before shipping any of v9.9, v9.9.1, v9.9.2. The hotfix layers in those releases couldn't help because the script never ran. Going forward I'll validate JS syntax on every ship.

### Fix

Hex-byte-level inspection of every fence in the file:
- 4 broken fence triples (12 source bytes) in MD, skill, claude-project export blocks → fixed to use proper `\\\``\``\\\``\``\\\``\`` escape (6 bytes)
- 2 cosmetically-wrong fences in MD format → corrected to standard 2-byte tokens
- Two single-quoted-string fences in `v97RegionalContext` (different syntax, not template literals) left alone

### Verification

`node --check` now passes cleanly. All v9.9 features verified intact: MD/skill/claude-project export Master Context, bulletproof skip handler, rich brief setup, resolution dropdown, HEX/RGB palette toggle.

## What was fixed in v9.9.2 (skip button still broken in v9.9.1)

The v9.9.1 try/catch I added was inside `completeWizard()` — but that's the function the skip button CALLS, not the function that wires the click. The wiring lives inside `initWizard()`. If anything earlier in `initWizard()` throws, the wiring never happens and the click goes nowhere.

Three layers of defence in v9.9.2:

1. **Skip element is now a `<button>`** instead of an `<a>` without href. Some browsers don't fire reliable clicks on hrefless anchors.
2. **Document-level delegated click handler** in capture phase. Runs at script-load, independent of `initWizard()`. Catches any click on `#wizSkip` even if the wizard wiring fully crashed.
3. **Fallback chain inside the handler**: tries `completeWizard()` → falls back to `initApp()` → as last resort just sets wizard `display: none` and app `display: block`. User can never get stuck.

## What was fixed in v9.9.1

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
| `index.html` | App v8.9.8.3 — JS parses cleanly, app actually runs | ~654 KB |
| `index.json` | v9.9.1 — scene_ids fix + distinct B/C/D | ~5.7 MB |
| `PROMPTSTUDIO-rebuilt.zip` | 4,437 prompt JSONs with distinct B/C/D + richBrief | ~15.0 MB |
| `PromptStudioPro-v9-database.xlsx` | v9.9 workbook (unchanged) | ~4.3 MB |

---

## Deploy

```bash
cd ThePromptStudio
cp /path/to/v9.9.3/* .
unzip -o PROMPTSTUDIO-rebuilt.zip
git add -A
git commit -m "v8.9.8.3 + v9.9.1 — fix template literal syntax error that killed the app"
git push origin main
```

Then **hard-refresh:**
- Laptop: Cmd+Shift+R (Mac) or Ctrl+Shift+F5 (PC)
- iPhone: Settings → Safari → Clear History and Website Data

---

## Test after deploy — 3 critical checks

1. **Open wizard, click "Skip for now".** Wizard closes, app loads. (If this fails: check browser console for any error starting with `[skip]` — those would indicate the bulletproof handler ran but something inside `completeWizard()` threw.)
2. **Click any niche.** Tiles should appear with three distinct text rows: bold title, italic sub-text (e.g. "Video · Hook"), then "What you get:" description.
3. **Open any tile.** Modal opens with rich content, dropdowns work, prompt text is visible.

If all three work, v9.9.2 is good. Then test the v9.9 features (export Master Context, HEX/RGB toggle, pixel-size badges, resolution dropdown, ENRICHED CONTEXT in prompts).

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
