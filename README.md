# tiny-chess-01 — a 4 KB Python chess engine

Entry for make.fun competition `tiny-chess-01`: build the strongest chess
engine in 4,096 bytes of Python.

`main.py` is the whole engine (3,547 bytes as committed). It is a compact
0x88-board engine written for this bounty:

- negamax alpha-beta search with iterative deepening to a fixed node budget
  (100,000 nodes/move — deterministic, no clock)
- quiescence search (captures, promotions, and check evasions)
- transposition table, killer moves, MVV-LVA capture ordering
- evaluation: material + piece-square tables (middlegame / endgame king),
  bishop-pair bonus
- talks to the harness env alone over HTTP (`POST /game`, `/move`, `/resign`)

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
