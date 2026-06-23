# World Cup 2026 Tracker

This repo powers wc26.eggpot.co.uk — a World Cup 2026 schedule, results and standings page.

## To refresh scores

> **⚠️ CRITICAL — READ BEFORE ADDING ANY SCORE ⚠️**
>
> Only record a score when ALL of the following are true:
> 1. **Match status is explicitly "Full Time" / "FT" / "Final"** — never use a live or in-progress score
> 2. **Both team names match the fixture exactly** — check `fixtures` in `scores.json` for the exact home/away pair
> 3. **The date matches** — verify the match was played on the date shown in the fixture, not a qualifier or friendly
> 4. **Cross-check at least two independent sources** (e.g. BBC Sport + ESPN, or FIFA.com + Sky Sports) before writing any result
>
> The automated routine has previously committed live in-progress scores (e.g. a 1-0 scoreline mid-match) and scores from a completely different competition. **If you cannot find two sources confirming the same final result for the exact fixture, do not add the score.**

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

## To refresh Top 10 stats

The `top10` array in `scores.json` powers the "Top 10" tab. Each entry has:
- `id` — unique key (never change)
- `label` — display name with emoji
- `unit` — column header (goals / assists / saves / etc.)
- `entries` — array of `{"name": "...", "team": "...", "value": number}`, sorted descending by value

When refreshing scores, also search for updated player stats and update `top10` entries:
- **goals** / **assists**: search official stats from FIFA.com, ESPN or fotmob — update after every matchday
- **saves**: best single-match save tally per goalkeeper in the tournament; update when a keeper makes a notable haul
- **shots / yellow_cards / dribbles / distance / touches**: populated from aggregated tournament stats (ESPN, fotmob, sofascore). These start empty and fill as data becomes available.
- **red_cards_team**: count of red cards received per team — update after any match with a sending off
- **red_cards_player**: list of all sent-off players; value = match ban length (Zwane=3 for violent conduct, others=1 for standard ban). Update after any red card.
- **touches_team_most** / **touches_team_least**: average or total ball touches per team from official stats pages
- **touches_player**: most touches by any individual player across the tournament

Leave `"entries": []` for any category where reliable data isn't yet available — the UI shows "Data coming soon" for empty categories.

## To update knockout teams/channels

After the group stage, update the `home`/`away` fields on knockout fixtures in `scores.json`
and fill in confirmed `channel` values. Commit and push directly to `main`.

## R32 bracket notes

- R32-5: E2 (Ivory Coast) vs I2 (Norway) — runner-up of Groups E and I
- R32-6: I1 (France) vs 3rd-place qualifier — winner of Group I
- "3rd Place*" slots in R32 fixtures are dynamically replaced in JavaScript by `assign3rds()` using the `THIRD_ELIGIBLE` lookup. Never manually set them.
- After each matchday, verify R32 fixture `home`/`away` names match actual group standings (i.e. G1=group winner, G2=runner-up). Update if a projected team name no longer matches.

## Files

- `index.html` — presentation only, fetches scores.json at page load
- `scores.json` — all data (static fixtures + dynamic results/standings/top10)
- `CLAUDE.md` — this file, read automatically by Claude Code
