[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

[![rank](https://make.fun/badge/tiny-chess-01/zargaan/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

[![reward](https://make.fun/badge/tiny-chess-01/zargaan/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)


# Tiny Chess — a 4 KB chess engine

`main.py` (4,094 bytes) is a complete chess engine in Python 3.13,
standard library only. It plays the make.fun tiny-chess-01 gauntlet
through the env protocol (`MAKEFUN_ENV_URL`): `POST /game` to poll,
`/move` with a UCI move, `/resign` when lost.

## Engine

- 0x88 board, negamax alpha-beta with quiescence search (captures,
  promotions, and all check evasions; no stand-pat while in check)
- Transposition table (Zobrist, seeded `random.Random`), killer moves,
  MVV-LVA capture ordering, repetition scored as a draw
- Evaluation mirrors the house formula: material (100/320/330/500/900),
  formulaic piece-square tables, bishop-pair bonus, king table blended
  middlegame/endgame by non-pawn material
- Iterative deepening to a fixed node budget (100k nodes, 60k after
  ply 120, 40k after ply 160) — deterministic, no clock, no randomness
- Packed as an LZMA (FORMAT_ALONE) + base85 payload with a tiny
  unpacker, so the whole engine fits the 4,096-byte cap

## Results (local practice trial-0, official scorer)

- 100k nodes, eval fixed: **Elo 1451** — 3 won, 2 drawn, 1 lost
  (practice floor is 1,154; house baseline sunfish 4K ≈ 1,365)
- Perft and incremental eval/hash verified against python-chess
  over random playouts (0 mismatches)

## Files

- `main.py` — the engine (the only file that runs)
- `README.md` — this file
- `LICENSE` — MIT
