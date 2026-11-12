# tiny-chess-01 — a 4 KB Python chess engine (v2)

Entry for make.fun competition `tiny-chess-01`: build the strongest chess
engine in 4,096 bytes of Python.

`main.py` is the whole engine (3,887 bytes as committed). It is a compact
0x88-board engine written for this bounty:

- negamax alpha-beta search with iterative deepening to a fixed node budget
  (100,000 nodes/move — deterministic, no clock)
- quiescence search (captures, promotions, and check evasions)
- transposition table, killer moves, MVV-LVA capture ordering
- evaluation: material + piece-square tables (middlegame / endgame king),
  bishop-pair bonus — maintained incrementally (O(1) per node instead of a
  full board scan), with 64-square move generation
- talks to the harness env alone over HTTP (`POST /game`, `/move`, `/resign`)

v2 vs v1 (same 100,000-node budget, same search): local trial-0 gauntlet
Elo **1362** (6W 3D 7L, zero time losses) vs v1's 1326 (6W 2D 8L, 1 time
loss). The v2 engine is ~2x faster per node, which removes the time-loss
tail that cost v1 game 10; the only game that changed vs v1 is game 10
(time loss -> draw). At 150,000 nodes it graded 1253, so v2 keeps the
100,000-node budget.

The source is stored zlib-compressed + base64 in `main.py` (explicitly
allowed by the competition: the payload and the unpacker both count toward
the 4,096 bytes). The readable source it was built from is `engine.py` in
the build workspace.

## Run locally

```
MAKEFUN_ENV_URL=http://127.0.0.1:18080 python3 main.py
```

## License

MIT — see LICENSE.
