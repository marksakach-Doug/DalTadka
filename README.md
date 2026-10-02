# Dal Tadka: Real Time

A small browser game about cooking dal in real time. Wash the grit, simmer the dal, bloom the spices, then serve. Pick your own pace: rush for a speed bonus or go slowly for a care bonus. Sloppy dal is punished hard.

## Play

Open the GitHub Pages link for this repo, or run it locally:

```
python3 -m http.server
```

then visit http://localhost:8000. (Opening `index.html` directly works for playing, but the leaderboard needs a server.)

## Leaderboard

Your best scores are always saved in your browser. To share a leaderboard with friends, use either option:

**Option A: Google Sheet (recommended)**

1. Create a Google Sheet. In row 1, type: `name`, `lentil`, `score`, `date` (one per column).
2. Go to Extensions > Apps Script, delete the default code, and paste in `leaderboard-backend.gs`.
3. Click Deploy > New deployment > Web app. Set "Execute as" to *Me* and "Who has access" to *Anyone*. Deploy and copy the URL.
4. In `index.html`, paste the URL into `const API=''` near the top of the script. Commit.

If you edit the script later, use Deploy > Manage deployments to publish a new version.

**Option B: `leaderboard.json`**

Leave `API` empty. After a run, copy the entry line the game shows into the `entries` list in `leaderboard.json` and commit. Use commas between entries and none after the last one.

Anyone with the link can add scores. It isn't secure, which is fine for friends.

## Files

- `index.html`: the whole game
- `leaderboard-backend.gs`: Google Sheet backend (Option A)
- `leaderboard.json`: file-based leaderboard (Option B)
