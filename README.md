# makefun-tiny-chess-01

A 4 KB Python chess engine for the make.fun tiny-chess-01 competition:
build the strongest chess engine in 4,096 bytes of Python.

`main.py` is the whole engine (standard library only). It plays the house
gauntlet through the env at `MAKEFUN_ENV_URL` (`POST /game`, `/move`,
`/resign`) and exits when every game is over.

## Engine

- 0x88 board, alpha-beta + quiescence (captures, promotions, check replies)
- Transposition table, killer moves, history heuristic, MVV-LVA ordering
- Null-move pruning and late-move reductions, check extensions
- Material + piece-square evaluation with a tapered king table and the
  bishop-pair bonus; repetition scored as a draw from the game history
- Fixed node budget per move (never a clock), so trials replay exactly

## Size

- `main.py` is at most 4,096 bytes as committed; the whole snapshot is at
  most 8,192 bytes and no file is over 4,096 bytes.
- MIT licensed; see `LICENSE`.

## Revisions

- En-passant captures are exempt from quiescence futility pruning: the
  victim is not on the target square, so the old test valued the capture
  at zero and could prune a winning en-passant capture.
- The bishop-pair bonus is 50 centipawns, the value most engines use.

## Practice results

- Trial 0 of the house gauntlet: 3 wins, 1 draw, 2 losses, Elo 1,364.
- Perft is correct on startpos, Kiwipete, en-passant, promotion and
  castling positions, and a 300-ply make/unmake round-trip restores the
  board, hash and side to move exactly.

## Grade it locally

From the competition kit directory, with Docker:

    node cli/makefun.mjs grade-local --kit . --bot <this repository> --driver docker

---

Built for make.fun — [join with my referral link](https://make.fun/r/kfz6780b)
