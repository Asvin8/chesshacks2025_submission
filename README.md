# ChessHacks 2025 Submission

A chess bot built for ChessHacks 2025. It scores positions with a small CNN evaluator (`src/data_generation/chess_cnn_eval.pt`, PyTorch) and searches with time-limited alpha-beta (`src/main.py`, python-chess). A FastAPI server (`serve.py`) exposes it over HTTP.

## Setup

```bash
git clone https://github.com/Asvin8/chesshacks2025_submission
cd chesshacks2025_submission
python3 -m venv .venv && source .venv/bin/activate
pip install torch --index-url https://download.pytorch.org/whl/cpu   # optional: CPU-only torch
pip install -r requirements.txt
```

## Run

```bash
python serve.py                 # listens on 0.0.0.0:5058 (override with SERVE_PORT)
```

## API

- `POST /` returns `{"running": true}`
- `POST /move` takes JSON `{"pgn": "<game so far>", "timeleft": <ms>}`. An empty `pgn` means the starting position.

```bash
curl -X POST localhost:5058/move -H 'Content-Type: application/json' \
  -d '{"pgn": "1. e4 e5 2. Nf3", "timeleft": 60000}'
```

Example response (abridged):

```json
{"move": "d8h4", "error": null, "time_taken": 618.8,
 "move_probs": {"d8h4": 0.12, "...": 0.0}, "logs": "..."}
```
