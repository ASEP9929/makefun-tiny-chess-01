# tiny-chess-01 entry

A chess engine in 4,096 bytes of Python (`main.py` is 4,058 bytes: an
lzma FORMAT_ALONE payload plus its unpacker). It plays the gauntlet
through the env at `MAKEFUN_ENV_URL`: `POST /game` serves the game in
play, `POST /move` plays a move in UCI.

## Engine

0x88 board, negamax with alpha-beta, PVS, transposition table, killer
moves, history heuristic, MVV-LVA ordering, null-move pruning,
futility pruning, and a capture-only quiescence search with check
evasions. Evaluation is material plus arithmetic piece-square terms,
king safety (phase-interpolated), and the bishop pair. Search is
iterative deepening to a fixed node budget (120,000 nodes a move
before ply 120, 60,000 after) with an 8 s wall-time cap per move —
never a clock for decisions, no randomness, no model calls, so a
replay plays the same moves. Original code, not a sunfish fork.

## Practice results (kit practice trials, local)

| trial | W-D-L | Elo | spare_plies |
|---|---|---|---|
| 0 | 6-4-6 | 1,396 | 838 |
| 1 | 2-7-7 | 1,214 | 182 |
| 2 | 1-8-7 | 1,173 | 87 |
| mean | | **1,261** | |

Baseline sunfish-4k practice: 1,326 / 1,464 / 1,290, mean 1,360.
This entry's mean (1,261) is below the baseline mean, so the Elo
gate is not met on practice trials. Main weakness: many draws by
threefold repetition (6–8 per trial) and losses to the strongest
house engines (nodes-1M/2M).

## Size

- `main.py`: 4,058 bytes (limit 4,096)
- Whole tree: under 8,192 bytes, no file over 4,096
- License: MIT (see `LICENSE`)
