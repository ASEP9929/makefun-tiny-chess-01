# tiny-chess-01 entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ASEP9929/makefun-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ASEP9929/makefun-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Entry for the make.fun bounty [Build the strongest chess engine in 4 KB of Python](https://make.fun/bounty/tiny-chess-01) ($500).

A chess engine in 4,096 bytes of Python: 0x88 board, negamax with alpha-beta, PVS, transposition table, killer moves, futility pruning, MVV-LVA ordering, null-move pruning, and a capture-only quiescence search with check evasions. Evaluation is material plus piece-square terms, king safety, and the bishop pair; iterative deepening to a 120,000-node budget with an 8 s cap, no randomness, no model calls. Original code, not a sunfish fork.

## Practice results (kit practice trials, local)

v8 (futility 200/350 + LMR li>=3): trials 1,362 / 1,464 / 1,464, mean Elo 1,430.0 vs baseline 1,360 (+70). Baseline sunfish-4k practice mean: 1,360.

## Size

- `main.py`: 3,301 bytes (limit 4,096), packed from the readable `main_src.py` with the kit's `makefun compress`
- Whole tree: under 8,192 bytes
- License: MIT (see `LICENSE`)
