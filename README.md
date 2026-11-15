# tiny-chess-01 entry

A chess engine in 4,096 bytes of Python (`main.py` is 4,061 bytes: an
lzma FORMAT_ALONE payload plus its unpacker). It plays the gauntlet
through the env at `MAKEFUN_ENV_URL`: `POST /game` serves the game in
play, `POST /move` plays a move in UCI.

## Engine

0x88 board, negamax with alpha-beta, PVS, transposition table,
history heuristic, MVV-LVA ordering, null-move pruning, and a
capture-only quiescence search with check evasions. Repetition
detection: the engine replays the game history on every call and
scores a position already seen twice as a draw (0) in the search,
so it stops shuffling into threefold draws. Killer moves and
futility pruning were removed to fit the repetition code in 4,096
bytes (measured: repetition +114 packed bytes; killers −32,
futility −62). Evaluation is material plus arithmetic piece-square
terms, king safety (phase-interpolated), and the bishop pair. Search
is iterative deepening to a fixed node budget (120,000 nodes a move
before ply 120, 60,000 after) with an 8 s wall-time cap per move —
never a clock for decisions, no randomness, no model calls, so a
replay plays the same moves. Original code, not a sunfish fork.

## Practice results (kit practice trials, local)

| trial | W-D-L | Elo | spare_plies |
|---|---|---|---|
| 0 | 6-2-8 | 1,326 | 309 |
| 1 | 4-2-10 | 1,173 | 510 |
| 2 | not run | — | — |
| mean (2 trials) | | **1,249.5** | |

Baseline sunfish-4k practice: 1,326 / 1,464 / 1,290, mean 1,360.
This entry's mean (1,249.5 over 2 trials) is below the baseline
mean, so the Elo gate is not met. Trial 2 was not run: the gate
needs a 3-trial mean above 1,360, which would require a trial-2 Elo
of 1,581 — far outside this engine's observed range — so it cannot
change the verdict. The repetition fix worked mechanically (draws
fell from 6–8 per trial to 2 per trial, wins rose), but removing
futility pruning and killer moves to fit it cost more Elo than the
fix gained. Short trial (5 games, white only): 3-2-0, Elo 1,525.

Next-iteration lead (measured, not yet built): renaming the long
global function names in the minified source to free single letters
saves 46 packed bytes with identical behaviour (verified: PS, move
generation, eval, and the repetition check all identical). That
almost pays for futility pruning back (62 bytes); combined with
dropping the history heuristic instead (55 bytes) it fits with
futility + killers kept. That variant would need 3 fresh practice
trials to judge.

## Size

- `main.py`: 4,061 bytes (limit 4,096)
- Whole tree: under 8,192 bytes, no file over 4,096
- License: MIT (see `LICENSE`)
