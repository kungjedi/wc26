# World Cup 2026 Tracker

This repo powers wc26.eggpot.co.uk — a World Cup 2026 schedule, results and standings page.

## To refresh scores

Search the web for all completed 2026 FIFA World Cup results, then update `scores.json`:

- Only modify the `results`, `standings` and `_meta.last_updated` fields
- Never modify `fixtures` or `groups`
- `results` format: fixture id → [home_score, away_score] e.g. `"A1": [2, 0]`
- Recalculate all `standings` from scratch based on the full results set
- Set `_meta.last_updated` to today's date and a matchday summary e.g. `"13 Jun 2026 · Matchday 2"`
- For each newly completed fixture, find the BBC Sport YouTube highlights and add the URL to `highlight_links` e.g. `"A1": "https://www.youtube.com/watch?v=..."`. BBC Sport videos always follow the format `HIGHLIGHTS - Team v Team | tagline | FIFA World Cup 2026` — search for that pattern on YouTube. Leave as `""` if not found.
- Also search for any new funny, bizarre or interesting World Cup stories and prepend them to the `funnies` array (newest first, with a `date` field e.g. `"Sat 13 Jun"`): `{"date": "...", "headline": "...", "summary": "...", "url": "..."}`
- Commit and push directly to `main` when done — this makes changes live immediately on the site

## Fixture IDs

Group stage fixtures are named by group + sequence: A1, A2... L6.
Knockout fixtures: R32-1 through R32-16, R16-1 through R16-8, QF-1 through QF-4, SF-1, SF-2, 3PO, FINAL.

## Fixing fixture data

Occasionally a fixture's `date` or `bst` may need correcting (e.g. a BST conversion error).

**Critical:** the `fixtures` array must always stay in strict chronological order by `date` + `bst`. The schedule page groups fixtures into date sections based purely on array order — if a fixture is out of sequence it will create a duplicate date header on the page. When editing a fixture's date, also move it to the correct position in the array.

BST conversion reminder: match times in North American venues are often late evening local time, which rolls into the next calendar day in BST (UTC+1). Always verify the BST date against the local kick-off time before committing.

## To update knockout teams/channels

After the group stage, update the `home`/`away` fields on knockout fixtures in `scores.json`
and fill in confirmed `channel` values. Commit and push directly to `main`.

## Files

- `index.html` — presentation only, fetches scores.json at page load
- `scores.json` — all data (static fixtures + dynamic results/standings)
- `CLAUDE.md` — this file, read automatically by Claude Code
