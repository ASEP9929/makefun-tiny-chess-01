# tiny-chess-01 entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ASEP9929/makefun-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ASEP9929/makefun-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Entry for the make.fun bounty [Build the strongest chess engine in 4 KB of Python](https://make.fun/bounty/tiny-chess-01) ($500).

A chess engine in 4,096 bytes of Python (`main.py` is 4,042 bytes: an
lzma FORMAT_ALONE payload plus its unpacker). It plays the gauntlet
through the env at `MAKEFUN_ENV_URL`: `POST /game` serves the game in
play, `POST /move` plays a move in UCI.

## Engine

0x88 board, negamax with alpha-beta, PVS, transposition table,
killer moves, futility pruning, MVV-LVA ordering, null-move pruning,
and a capture-only quiescence search with check evasions. Repetition
detection: the engine replays the game history on every call and
scores a position already seen twice as a draw (0) in the search,
so it stops shuffling into threefold draws. The history heuristic
was dropped to fit futility + killers back (55 bytes); long global
function names in the minified source are renamed to single letters
(48 packed bytes saved, behaviour verified identical: PS, move
generation, eval, repetition check, killers). Evaluation is material
plus arithmetic piece-square terms, king safety (phase-interpolated),
and the bishop pair. Search is iterative deepening to a fixed node
budget (120,000 nodes a move before ply 120, 60,000 after) with an
8 s wall-time cap per move — never a clock for decisions, no
randomness, no model calls, so a replay plays the same moves.
Original code, not a sunfish fork.

## Practice results (kit practice trials, local)

| trial | W-D-L | Elo | spare_plies |
|---|---|---|---|
| 0 | 7-4-5 | 1,464 | 591 |
| 1 | 6-3-7 | 1,362 | 404 |
| 2 | 1-6-9 | 1,087 | 0 |
| mean (3 trials) | | **1,304.3** | |

Baseline sunfish-4k practice: 1,326 / 1,464 / 1,290, mean 1,360.
Mean diff −55.7; 90% paired t-interval on the per-trial diffs
[−384.4, +273.1], lower bound below 0 — the Elo gate is not met.
v5 improved on v4c (mean 1,249.5 over 2 trials, +54.8) but trial 2
(1,087, spare_plies 0) shows the variance is still too large.
Short trial (5 games, white only): 4-1-0, Elo 1,687 (v4c: 1,525).

## Size

- `main.py`: 4,042 bytes (limit 4,096)
- Whole tree: under 8,192 bytes, no file over 4,096
- License: MIT (see `LICENSE`)
