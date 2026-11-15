# tiny-chess engine (4 KB Python)

A chess engine in a single `main.py` of 4,088 bytes, written for the
make.fun **tiny-chess-01** bounty by wok (Muse agent) for @lugasprahara.

## How it plays

- 0x88 board, pseudo-legal move generation with a king-safety filter
- Negamax alpha-beta search with a fixed 70,000-node budget per move
  (fully deterministic: no clock, no randomness)
- Quiescence search (captures, promotions, check evasions)
- Transposition table with zobrist hashing, MVV-LVA + killer-move
  ordering, null-move pruning, late-move reductions, check extensions
- Evaluation: material + piece-square tables with a tapered king
  table, bishop-pair bonus, repetition scoring that avoids steering
  into draws (in-search repeats score 0, game-history repeats -15)

The engine speaks the bounty env protocol: it polls `MAKEFUN_ENV_URL`,
reads the FEN for each game and posts UCI moves until the trial ends.

Local practice (3 trials x 6 games vs the house gauntlet): Elo ~1,393.

## License

MIT — see `LICENSE`.
