# Tiny Chess Engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/baddairdrop-svg/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/baddairdrop-svg/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A deterministic chess engine in at most 4,096 bytes of Python, built for the
[make.fun](https://make.fun) tiny-chess-01 bounty and entered by
@badd-airdrop2.

`main.py` is the whole engine, packed from the readable source `engine.py`
with the official `makefun compress` tool. The engine plays through the
make.fun environment protocol (`MAKEFUN_ENV_URL`).

## How it plays

- 120-square board with full legal move generation: castling, en passant,
  promotions, check evasion.
- Negamax alpha-beta search with iterative deepening (depths 1-6) under a
  fixed budget of 54,000 nodes per move — fully deterministic, never a clock.
- Quiescence search over captures and check evasions, with delta pruning.
- Transposition table with exact/lower/upper bounds, PVS, late-move
  reductions, null-move pruning, check extensions, aspiration windows.
- Futility pruning at depths 1-2 with tuned margins (100/200 centipawns).
- Killer moves and a history heuristic for move ordering.
- Repetition-aware search: in-tree repeats and game-history threefold
  positions score as draws, so the engine steers for wins it can actually
  convert and secures draws it would otherwise throw away.
- Evaluation: material plus Michniewski-style piece-square tables with
  separate middlegame/endgame king tables and a bishop-pair bonus.

## Validation

- Perft-exact on five standard test positions (startpos, Kiwipete, pawn
  endgame, promotion, position 5).
- Local final-format simulation (official kit generator and scorer, six
  paired 8-game instances against the kit anchor ladder): mean Elo 1,469,
  worst instance 1,253, 27.0/48 points.
- Official kit practice trial: Elo 1,451.

## License

MIT — see `LICENSE`.
