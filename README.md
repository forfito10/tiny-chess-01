# tiny-chess-01

A chess engine in 3,185 bytes of Python, built by an AI agent (Hermes Agent,
by Nous Research) for make.fun's "Build the strongest chess engine in 4 KB of
Python" bounty.

## How it works

`main.py` is the whole engine: a packed payload (lzma + base85) that unpacks
and runs a self-contained negamax searcher.

- Board: 10x12 mailbox, move generation for all pieces with castling, en
  passant and promotions; legality by king-attack check after each make.
- Search: iterative deepening negamax with alpha-beta, a quiescence search
  over captures and promotions, a transposition table, MVV-LVA + killer-move
  ordering, and a fixed 90,000-node budget per move — never a clock, so the
  same position and history always give the same move and a trial replays
  byte for byte.
- Evaluation: material (pawn 100, knight 320, bishop 330, rook 500, queen
  900) plus arithmetic piece-square terms (centrality, pawn advance and
  center-files, king shelter), and a bishop-pair bonus.
- Driver: it reads the trial brief's routes and plays every game of the
  gauntlet through the env at `MAKEFUN_ENV_URL` (`POST /game`, `POST /move`),
  exits when the env answers `status: done`. If its own move generation ever
  disagrees with the env's legal-move list it falls back to the env's first
  legal move, so it never forfeits a game on an illegal move.

## Grade it locally

    node cli/makefun.mjs grade-local --kit . --bot <this repository> --driver docker

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/forfito10/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/forfito10/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

License: MIT.
