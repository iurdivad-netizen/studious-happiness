# Scorched Earth

A fan-made tribute to the classic DOS artillery game *Scorched Earth* (Wendell Hicken, 1991), rebuilt from scratch for the browser. It is an independent look-alike: it contains no code or assets from the original, and it is not affiliated with or endorsed by the original author or publisher. Two to ten tanks sit on procedurally generated terrain and take turns lobbing shells at each other. Pick an angle and power, account for gravity and wind, and fire. Craters reshape the ground, and one direct hit is lethal.

The whole game is a single self-contained file, [`index.html`](index.html). It has no build step, no dependencies and no network access. Sound and music are synthesised in the browser with the Web Audio API.

## Running it

Open `index.html` in a modern desktop or mobile browser.

Or serve it locally:

```bash
python3 -m http.server 8000
```

Then visit <http://localhost:8000>.

## How to play

1. On the setup screen, choose the players (human or AI, with a difficulty for each AI), how many matches to play, the planet and the random-event frequency, then press **START GAME**.
2. On your turn, set **angle** and **power**, choose a **weapon**, and press **FIRE!**.
3. Last tank standing wins the match. After each match there is an **arsenal shop** where you spend your winnings.
4. When the final match ends, the player with the highest score wins the tournament.

### Controls

| Action | Keyboard | Touch / mouse |
|---|---|---|
| Adjust angle | `←` `→` (or `A` `D`) | ± buttons |
| Adjust power | `↑` `↓` (or `W` `S`, `[` `]`) | ± buttons |
| Fine adjustment | Hold `Shift` (1 step instead of 2) | – |
| Cycle weapon | `Q` / `E` | Weapon dropdown |
| Fire | `Space` or `Enter` | **FIRE!** button |

The game screen also has toggles for sound effects, music and its volume, and a **FAST** mode that speeds up animations and AI thinking. **✕ MENU** abandons the game.

## Weapons

You start every match with unlimited Baby Missiles. Everything else is bought in the shop between matches.

| Weapon | Cost | Blast radius | Behaviour |
|---|---:|---:|---|
| Baby Missile | free | 14 | Basic shell. Replenished every match. |
| Missile | $100 | 38 | Bigger blast. |
| MIRV | $150 | 24 | Climbs as one shell, then splits into 5 warheads at the top of its arc. |
| Napalm | $200 | 60 | Scatters 8 smaller fragments that each explode where they land. |
| Nuke | $500 | 120 | Enormous blast. |
| Dirt Ball | $50 | 44 | Deals no damage. Adds terrain instead, for burying or shielding. |
| Roller | $100 | 26 | Lands and rolls downhill before detonating. |
| Funky Bomb | $250 | 30 | Bursts into 7 coloured bomblets that each explode where they land. |
| Leap Frog | $150 | 30 | Detonates, then hops on in its direction of travel for 3 more blasts. |

## Scoring and economy

- Every hit on an opponent is a kill. It pays **3× the weapon's damage in cash and 1× in points**, plus a **$500 and 200-point** kill bonus. A Nuke kill is worth $800 and 300 points; a Baby Missile kill is worth $575 and 225 points.
- Winning a match is worth **+500 points**.
- Cash carries across matches and buys weapons. Purchased weapons also carry over.
- Hitting yourself kills you and earns nothing.

## Planets

Gravity and wind depend on the arena. In **Fixed** mode every match uses the planet you pick. In **Random** mode each match gets a new one.

| Planet | Gravity | Wind |
|---|---:|---|
| Moon | 0.17g | none |
| Mars | 0.38g | light |
| Earth | 1.00g | normal |
| Jupiter | 2.53g | fierce |
| Titan | 0.14g | gusty haze |

Launch speed is scaled to gravity and field width so that a given power setting reaches a similar distance on every planet.

## Random events

Each turn change can trigger an event. Choose **Off**, **Rare** (about 1 turn in 10), **Occasional** (about 1 in 5) or **Chaos** (about 2 in 5) on the setup screen.

- **Lightning strike**: a bolt hits near a tank, or scorches the dirt.
- **Asteroid impact**: a large crater with heavy damage.
- **Earthquake**: terrain slumps and any tank left hanging drops.
- **Wind storm**: the wind spikes.
- **Supply drop**: a crate parachutes in with ammo and cash for a random tank.

Events never award cash or points to anyone, but their damage can still kill.

## AI opponents

Computer players come in four levels. Higher levels aim more accurately, compensate for wind, and buy and use stronger weapons.

| Level | Aim error (angle / power) | Wind awareness | Favoured weapons |
|---|---|---:|---|
| Recruit | ±25° / ±20 | none | Baby Missile only |
| Soldier | ±15° / ±10 | 50% | Leap Frog, Roller, Missile |
| Veteran | ±6° / ±4 | 85% | Funky Bomb, Napalm, MIRV |
| General | ±2° / ±1 | full | Nuke, Funky Bomb, Napalm, MIRV |

## Settings that persist

Sound on/off, music on/off and music volume are saved in `localStorage`. Everything else resets when the page reloads.

## Project layout

```
index.html   # markup, styles and all game code (canvas rendering, physics, AI, audio)
```

Tuning constants live near the top of the script in `index.html`: `WEAPONS`, `PLANETS`, `AI_LEVELS` and `RANDOM_EVENTS`. Adding a weapon is mostly a new entry in `WEAPONS` plus a case in the sound manager and the projectile or explosion code if it has special behaviour.

## Contributing

There are no tests or build tooling yet. Check changes by playing a few rounds in a desktop and a mobile-sized browser window, including at least one match on a high-gravity planet and one with random events set to Chaos.
