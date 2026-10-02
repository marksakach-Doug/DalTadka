# Dal Tadka: Real Time

A small browser game about cooking dal in real time. Wash the grit, simmer the dal, bloom the spices, then serve. Rush for a speed bonus or go slowly for a care bonus. Sloppy dal is punished hard.

## Play

Open the GitHub Pages link for this repo, or run locally with `python3 -m http.server` and visit http://localhost:8000.

## Shared leaderboard

Scores are stored in a Google Sheet.

1. Create a Google Sheet. In row 1, type `name`, `lentil`, `score`, `date` (one per column).
2. Open Extensions > Apps Script, delete the default code, and paste in `leaderboard-backend.gs`.
3. Click Deploy > New deployment > Web app. Set "Execute as" to *Me* and "Who has access" to *Anyone*. Deploy and copy the URL.
4. Paste the URL into `const API=''` near the top of the script in `index.html`, then commit.

If you edit the script later, use Deploy > Manage deployments > New version. If you replace `index.html`, make sure the `API` URL is still there.

Anyone with the link can add scores. It isn't secure, which is fine for friends.

## Files

- `index.html`: the whole game
- `leaderboard-backend.gs`: Google Sheet backend
