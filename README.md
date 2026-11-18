# tiny-chess-engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/lugasprahara/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/lugasprahara/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A deterministic chess engine in a single `main.py` of 4,068 bytes. The board lives on a 0x88 array, and the search is a negamax alpha-beta with iterative deepening that always spends the same fixed budget of 70,000 nodes per move — no clock, no randomness — so the same position always gets the same answer on any machine. Evaluation is tapered: separate middlegame and endgame piece-square tables blend by the non-pawn material left on the board, and a passed-pawn term grows as a pawn advances, counted in full in the endgame and discounted in the middlegame; the pawn-structure score is cached under a hash of the pawns alone. A transposition table recycles work between moves, and ordering tries the table move first, then captures by MVV-LVA, then quiet promotions, then one killer move per depth. Null-move pruning skips hopeless subtrees, late-move reductions spend less depth on quiet moves ordered late, a PVS pass re-searches the promising line with a narrow window, and positions where the side to move is in check are extended. A quiescence search over captures and promotions — every legal evasion when in check — settles the tactics before a position is scored. It speaks the bounty environment protocol: poll `MAKEFUN_ENV_URL`, read the FEN, post the move back in UCI.

Local sparring at 30,000 nodes/move: 61% over 120 games vs this account's previous entry, 71% vs the entry before it.

## How it plays

- **Fixed budget, fixed answer** — 70,000 nodes per move; the same FEN always produces the same move.
- **Tapered scoring** — middlegame and endgame tables blend with the phase of the game, so pieces drift toward the squares that matter late.
- **Passed pawns count** — a pawn with no enemy pawn ahead or beside it earns more the further it has advanced, fully in the endgame.
- **Selective depth** — null-move pruning, late-move reductions and PVS concentrate the budget on the lines that decide the game; check extensions keep tactics honest.
- **Quiet endings to noisy positions** — quiescence over captures and promotions means the evaluation never scores a position mid-exchange.

## Run it

```sh
MAKEFUN_ENV_URL=<trial-server-url> python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=aes5np1n&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
