# Fake Account Detector

A Flask-based web app that analyzes Instagram account signals and predicts whether an account is likely fake or real.

## Project Overview and Features

- Web UI for manual account signal input
- Optional Instagram profile fetch by username
- ML model prediction (`Fake` or `Real`)
- Verdict generation with risk score and reasoning
- Training script for rebuilding the model from dataset CSV

## Project Structure

- `app.py`: Flask server and API routes
- `model.py`: model training script
- `utils/features.py`: feature extraction for inference
- `utils/verdict.py`: confidence-to-verdict logic
- `utils/instagram_fetch.py`: live Instagram profile fetch logic
- `templates/index.html`: main page
- `static/style.css`: UI styles
- `static/app.js`: frontend logic
- `data/instagram_dataset.csv`: training dataset
- `model/account_model.pkl`: trained model artifact
- `model/metrics.json`: training metrics

## Requirements

- Python 3.14+
- Linux/macOS/Windows

Dependencies are declared in `pyproject.toml`.

## Setup Instructions (uv Recommended)

### Option 1: Using `uv` (recommended)

```bash
uv sync
```

### Option 2: Using `pip`

```bash
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

Note: if your environment does not include a `requirements.txt`, use `uv sync` from Option 1.

## Model Training Commands (using `model.py`)

Run training with defaults:

```bash
python model.py
```

Custom paths:

```bash
python model.py \
  --data data/instagram_dataset.csv \
  --model-out model/account_model.pkl \
  --metrics-out model/metrics.json
```

## Temporary Graphs and Tables

This project now includes a temporary reporting script to generate:

- Model graph (feature importance)
- Model training data table preview
- Training vs testing data graph
- Accuracy graph
- Model accuracy summary JSON

Script path:

- `temporary/generate_model_reports.py`

Run it:

```bash
python temporary/generate_model_reports.py
```

Output folder (default):

- `temporary/model_reports/`

Generated files:

- `model_graph_feature_importance.png`
- `model_train_data_table.csv`
- `model_train_data_table.md`
- `model_train_data_table.png`
- `training_vs_testing_data_graph.png`
- `accuracy_graph.png`
- `model_accuracy_summary.json`

Current model accuracy (from `model/metrics.json`):

- `97.07%`

## App Run Instructions (using `app.py`)

```bash
python app.py
```

By default, server runs on:

- `http://127.0.0.1:5000`

## API Endpoint Summary

### `POST /api/analyze`

Analyzes account metadata and returns prediction + verdict.

Request JSON:

```json
{
  "username": "example_user",
  "bio": "some bio",
  "followers_count": 120,
  "following_count": 300,
  "media_count": 20,
  "has_profile_pic": 1
}
```

Response fields include:

- `prediction`
- `confidence`
- `verdict`
- `risk_score`
- `reasoning`

### `POST /api/fetch-instagram`

Fetches profile metadata for a username.

### `GET /health`

Simple health check endpoint.

## Troubleshooting

### Port already in use (`5000`)

If you see "Port 5000 is in use", either stop the existing process or run on another port.

Quick Linux check:

```bash
lsof -i :5000
```

Then stop the PID if needed:

```bash
kill <PID>
```

### Model load error on startup

Make sure `model/account_model.pkl` exists. If not, run:

```bash
python model.py
```

## Practical Notes on Usage Limits

- This tool is a screening assistant, not final identity verification.
- Predictions depend on training data quality and may not generalize to all account patterns.
- Live profile fetch can fail for private, blocked, rate-limited, or region-restricted profiles.
- Confidence scores indicate model certainty, not guaranteed truth.
- Always perform manual review before taking moderation, compliance, or legal action.
