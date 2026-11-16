# tiny-chess-engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/lugasprahara/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/lugasprahara/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A deterministic chess engine squeezed into a single `main.py` of 4,093 bytes. The board lives on a 0x88 array, and the search is a negamax alpha-beta that always spends the same fixed budget of 70,000 nodes per move — no clock, no randomness — so every position gets exactly the same answer on every run, on any machine. A transposition table recycles work between moves, and move ordering tries the table move first, then killers, then quiet moves ranked by a gravity-updated history heuristic, so the alpha-beta window closes early. Adaptive null-move pruning skips hopeless subtrees, late-move reductions spend less depth on moves that look unpromising, and an interior PVS pass re-searches the promising line with a narrow window. When the main search runs out of depth, a quiescence search over captures and quiet promotions settles the tactics before the position is scored. Evaluation combines material with piece-square tables, a passed-pawn bonus table, and an isolated-pawn penalty. The engine speaks the bounty environment protocol: it polls `MAKEFUN_ENV_URL`, reads the FENs the trial server deals, and posts its moves back in UCI notation.

## How it plays

- **Fixed budget, fixed answer** — 70,000 nodes per move, counted deterministically; the same FEN always produces the same move.
- **Fast cutoffs** — TT move, killers, and gravity history ordering put the likely best moves first so alpha-beta prunes hard.
- **Selective depth** — null-move pruning and late-move reductions skip or shrink the lines that don't matter, while PVS gives the principal variation a second, tighter look.
- **Quiet endings to noisy positions** — quiescence over captures and quiet promotions means the evaluation never scores a position mid-exchange.
- **Pawn-structure aware** — passed pawns are rewarded as they advance, isolated pawns are taxed, on top of piece-square tables for every piece.

## Run it

```sh
MAKEFUN_ENV_URL=<trial-server-url> python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=aes5np1n&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
