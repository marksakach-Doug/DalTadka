# Dal Tadka 🍲

A solo browser card game about cooking the perfect dal. Wash out the stones, boil the lentils, then nail a hot-pan tadka — all with a standard 52-card deck as your toolset. No luck-only runs: every phase is risk vs. reward.

Built with AI coding assistance. Fully self-contained — one HTML file, no build step, no dependencies.

## Play

Open `index.html` in any modern browser. That's it.

No server, no install:

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

### Publish with GitHub Pages

1. Push the `DalTadka/` folder contents to a GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)` (or `/DalTadka`), and save.

## How it works

You get 5 turns per phase, a 6-card hand, and a live flaw evaluation so you always know where the dish stands.

Card strengths: `A–4 = 1`, `5–9 = 2`, `10–K = 3`

| Card | Phase 2: Boiling | Phase 3: Tadka |
|---|---|---|
| Black ♠♣ (Fire) | 🔥 Heat +/− face value | 🔥 Heat +/− face value, 🌿 Spice +face value |
| Red ♥♦ | 💧 Water +face value, Heat −1 | 🧈 Richness +face value, Heat −1 |

### Phase 1 — Washing 🌊

Basin starts with 10 dal + 5 stones (15 items), 5 turns.

| Action | Draw | Strainer catches |
|---|---|---|
| Gentle | 2 | 2 dal |
| Standard | 3 | 2 dal |
| Reckless | 5 | 1 dal |

Draw more than the strainer catches and you spill dal. Spilled dal shrinks your Body, which shrinks your Buffer and tightens your boiling window. Leftover stones become grit. Finishing early banks efficiency bonus.

Derived stats:

- Body = `10 − spilled`
- Buffer (spoils dilution) = `2` if Body ≥ 8, `1` if Body ≥ 5, else `0`
- Cooks needed = `ceil(Body / 3)`
- Ideal water = `cooks needed` to `cooks needed + 1`

### Phase 2 — Boiling 🍲

Pot starts at Heat 2, Water 5, Cooked 0.

- Heat ≥ 3: +1 Cooked per turn, −1 Water
- Heat ≥ 5: +1 Cooked per turn, −2 Water
- 💧 Water cards add water but drop Heat −1

Hazards:

| Hazard | Trigger | Receive | React |
|---|---|---|---|
| Boiled Dry | Water ≤ 0 | Spoil 2, Water → 1 | +2 Water, −2 Heat |
| Boil Over | Heat ≥ 6 | Spoil 1, Heat → 4 | −2 Heat, averted |

You can also Finish Early for efficiency bonus (only counts if the dal is actually cooked).

### Phase 3 — The Tadka 🔥

Pan starts at Heat 2. Targets: Flavor `4–5`, Richness `2–3`.

Heat economy is 1:1 — the pan retains heat:

- 🔥 Fire: ±face value
- 🧈 Ghee: +Richness, Heat −1
- 🌿 Spice: Heat −1, outcome depends on heat *before* playing:
  - ≤ 3: too cold, no flavor
  - 4: perfect bloom, +face value flavor
  - 5: scorch, +face value +1 flavor, Spoil 1 hazard
  - 6+: burn, +1 flavor, Spoil 2 hazard
- 🥄 Stir (pass): Heat −1
- Pan Fire hazard at Heat ≥ 6

## Scoring — flaws win

Lower is better. Live evaluation runs the whole game.

| Source | Flaws |
|---|---|
| Spoils | each `max(0, spoil − Buffer)` |
| Grit | `stones × max(0, 3 − Buffer)` |
| Water | distance outside ideal window |
| Raw | `cooks needed − cooked` (min 0) |
| Flavor | distance outside 4–5 |
| Richness | distance outside 2–3 |
| Efficiency | `−floor(turns saved / 2)` |

Final ratings:

| Flaws | Verdict |
|---|---|
| ≤ 0 | Delicious! (Perfect) |
| ≤ 3 | Excellent |
| ≤ 6 | Satisfactory |
| ≤ 9 | Needs practice |
| 10+ | Kitchen disaster |

The end screen also roasts (or praises) your worst trait — watery lentil tea, ghee swamp, gravel crunch, and friends.

## Strategy tips

- Don't get greedy in the wash. Reckless flushes stones fast but every spilled dal narrows your boil window for the rest of the run.
- Camp Heat 3–4 while boiling. Heat 5 cooks no faster but doubles evaporation.
- Enter the tadka at Heat 2, fire up once to 4, *then* spice. Chaining ghee → spice without refiring is how blooms fail.
- React is usually right for Boiled Dry / Boil Over unless your Buffer can eat the spoil.
- Banked early turns are worth half a flaw each — finishing a phase 2 turns early offsets a minor seasoning miss.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole game. Markup, styling, and vanilla JS engine in one file. |
| `README.md` | This file. |

## Tech / tuning notes

- Vanilla JS + CSS, no frameworks, no assets. Game state lives in one `gameState` object; `updateUI()` re-renders HUD, tracks, and tools each turn.
- Balance: pan retains heat in Phase 3 (fire plays at face value; only ghee/spice/stir cost 1 heat). Previously an extra end-of-turn cooldown made Val-1 fire cards useless.
- Accessibility pass: 15px minimum body font, larger touch targets, track titles kept compact so `POT HEAT / WATER [x–y] / COOKED [x+]` don't overlap.
