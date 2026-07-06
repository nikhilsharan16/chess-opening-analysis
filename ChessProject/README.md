# Top-10 Chess Players: Opening Analysis

An analysis of the chess openings played by 10 top players — Carlsen, Caruana,
Erigaisi, Firouzja, Giri, Gukesh, Keymer, Nakamura, Praggnanandhaa, and Wei —
built from their PGN game archives.

## What this does

1. **Convert** each player's raw `.pgn` game archive into a `.csv` (one row per game) using [`python-chess`](https://python-chess.readthedocs.io/).
2. **Load** the ECO (Encyclopaedia of Chess Openings) reference database, which maps opening move sequences to ECO codes and names.
3. **Detect** each game's opening by matching its first few moves against the ECO database, using a prefix-bucketed lookup for speed.
4. **Tag** every game with its ECO code + opening name, saved as `<Player>_with_openings.csv`.
5. **Compare** opening choices across all 10 players (per-player breakdowns + an ECO-family distribution chart).

Everything lives in [`notebooks/analysis.ipynb`](notebooks/analysis.ipynb).

## Project structure

```
ChessProject/
├── notebooks/
│   └── analysis.ipynb          # full pipeline: PGN -> CSV -> opening detection -> analysis
├── data/
│   ├── chess_games/
│   │   ├── <Player>.pgn                # raw game archive
│   │   ├── <Player>.csv                # converted, one row per game
│   │   └── <Player>_with_openings.csv  # + detected ECO code & opening name
│   └── eco/
│       └── eco[A-E].json       # ECO opening reference database
├── tools/
│   └── pgn-extract.exe         # third-party PGN utility (see below)
├── requirements.txt
└── README.md
```

## Running it

```bash
pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb
```

Run all cells top to bottom. The notebook uses paths relative to its own
location, so it works regardless of where the repo is cloned — no path
edits needed. PGN → CSV conversion is skipped automatically if an up-to-date
CSV already exists.

## Notes on the data & tools

- **ECO database** (`data/eco/*.json`): a standard opening reference dataset
  (move sequence → ECO code → opening name), used purely as a lookup table
  for opening detection.
- **`tools/pgn-extract.exe`**: [pgn-extract](https://www.cs.kent.ac.uk/people/staff/djb/pgn-extract/)
  by David J. Barnes — a third-party, open-source PGN utility used for some
  raw PGN preprocessing/filtering before the notebook's conversion step. It's
  a prebuilt Windows binary; on macOS/Linux, install pgn-extract via your
  package manager or build it from source instead.
- Opening detection isn't 100% — some games are too short, transpose into an
  opening from an unusual move order, or don't have a clean ECO match. Typical
  detection coverage across these players is roughly 70–90% of games.

## Possible next steps

- Extend the ECO-family comparison chart with win-rate breakdowns per opening.
- Add per-player opening trends over time (openings tend to shift year to year).
- Parallelize the PGN→CSV conversion for faster runs on the larger archives (Carlsen, Nakamura).
