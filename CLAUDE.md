# World Cup 2026 Tracker

This repo powers wc26.eggpot.co.uk — a World Cup 2026 schedule, results and standings page.

## To refresh scores

Search the web for all completed 2026 FIFA World Cup results, then update `scores.json`:

- Only modify the `results`, `standings` and `_meta.last_updated` fields
- Never modify `fixtures` or `groups`
- `results` format: fixture id → [home_score, away_score] e.g. `"A1": [2, 0]`
- Recalculate all `standings` from scratch based on the full results set
- Set `_meta.last_updated` to today's date and a matchday summary e.g. `"13 Jun 2026 · Matchday 2"`
- Commit and push directly to `main` when done — this makes changes live immediately on the site

## Fixture IDs

Group stage fixtures are named by group + sequence: A1, A2... L6.
Knockout fixtures: R32-1 through R32-16, R16-1 through R16-8, QF-1 through QF-4, SF-1, SF-2, 3PO, FINAL.

## To update knockout teams/channels

After the group stage, update the `home`/`away` fields on knockout fixtures in `scores.json`
and fill in confirmed `channel` values. Commit and push directly to `main`.

## Files

- `index.html` — presentation only, fetches scores.json at page load
- `scores.json` — all data (static fixtures + dynamic results/standings)
- `CLAUDE.md` — this file, read automatically by Claude Code
