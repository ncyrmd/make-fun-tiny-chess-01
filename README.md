# tiny-chess-01 — a 4 KB Python chess engine (v5)

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/ncyrmd/make-fun-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/ncyrmd/make-fun-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

Entry for make.fun competition `tiny-chess-01`: build the strongest chess
engine in 4,096 bytes of Python. Bounty: https://make.fun/bounty/tiny-chess-01

`main.py` is the whole engine (4,031 bytes as committed). It is a compact
0x88-board engine written for this bounty:

- negamax alpha-beta search with iterative deepening to a fixed node budget
  (150,000 nodes/move — deterministic, no clock)
- principal variation search (PVS): full window on the first move, null
  window on the rest, re-search on fail-high
- null-move pruning (depth >= 3, static eval >= beta, not in check, with
  non-pawn material on the board; reduce 3 plies)
- late-move reductions (LMR): quiet moves after the first few are searched
  at reduced depth and re-searched at full depth on fail-high
- history heuristic: a 131,072-entry history table orders quiet moves
  (`+Hh[x]//64`) and is updated on beta cutoffs (`Hh[g] += d*d`)
- quiescence search (captures, promotions, and check evasions)
- transposition table, killer moves, MVV-LVA capture ordering
- evaluation: material + piece-square tables, bishop-pair bonus —
  maintained incrementally (O(1) per node), with 64-square move generation
- talks to the harness env alone over HTTP (`POST /game`, `/move`, `/resign`)

v5 vs v4 (same search, node budget 100,000 -> 150,000, plus history
heuristic + LMR, freed by code golfing): local trials on the current kit
(8 games each) — trial-0 Elo **1326** (2W 3D 3L), trial-1 Elo **1464**
(3W 3D 2L). Head-to-head vs v4: **6.0/8** (2.5/4 + 3.5/4). v5 is ~2x
slower per move than v4 (avg ~1.9s vs ~1.0s) because of the larger node
budget, but wins the head-to-head.

Dev-grade target: beat v4's Elo 1451.

v4 vs v2 (same 100,000-node budget): 1396 vs 1326 and 1464 vs 1326;
mean +104 Elo over the two trials.

The source is stored zlib-compressed + base64 in `main.py` (explicitly
allowed by the competition: the payload and the unpacker both count toward
the 4,096 bytes). The readable source it was built from is `engine5.py` in
the build workspace.

## Run locally

```
MAKEFUN_ENV_URL=http://127.0.0.1:18080 python3 main.py
```

## License

MIT — see LICENSE.
