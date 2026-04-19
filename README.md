# Glorbus — Real-Time Focus & Fatigue Detector

A webcam-based tool that monitors your focus and eye fatigue in real time using head pose estimation and blink rate analysis.

## What it does

- **Focus tracking** — estimates head pose (yaw, pitch, roll) via MediaPipe face landmarks and scores how aligned your gaze is with the screen. States: `FOCUSED`, `BORDERLINE`, `DISTRACTED`
- **Fatigue tracking** — measures your blink rate and compares it against a personal baseline. Flags eye strain (too few blinks) or fatigue (too many), and signals `BREAK NEEDED` after 3 minutes of sustained abnormal rate
- **Camera side detection** — automatically detects whether your webcam is left, right, or center-mounted at startup and adjusts the expected head angle accordingly
- **HUD overlay** — renders all stats directly on the video feed: focus state + score bar, pose angles, EAR value, blink rate vs. baseline, and calibration progress

## Requirements

- Python 3.10+
- Webcam

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Usage

```bash
python main.py
```

Press `Q` to quit.

On startup, hold still for ~3 seconds while the camera side is auto-detected. Fatigue calibration runs for the first 2 minutes of face-visible time to establish your personal blink baseline.

## Configuration

All tunable parameters are in `config.py`:

| Parameter | Default | Description |
|---|---|---|
| `EMA_ALPHA` | 0.10 | Score smoothing (lower = smoother) |
| `SCORE_FOCUSED_THRESHOLD` | 0.55 | Score above which state is FOCUSED |
| `BASELINE_ACTIVE_SECONDS` | 120 | Seconds of face time to calibrate blink baseline |
| `FATIGUE_MULTIPLIER` | 1.4 | Blink rate ratio to trigger fatigue warning |
| `BREAK_SUSTAINED_SECONDS` | 180 | Seconds of abnormal blink rate before BREAK NEEDED |

## Dependencies

- [OpenCV](https://opencv.org/) — video capture and rendering
- [MediaPipe](https://mediapipe.dev/) — face landmark detection
- [NumPy](https://numpy.org/) — pose math and EAR calculation
