# Virtual Mouse

A webcam-based virtual mouse that lets you control the cursor using hand gestures. It uses MediaPipe for hand tracking, OpenCV for video capture, and PyAutoGUI to drive mouse and keyboard actions.

**Highlights**
- Real-time hand tracking from a single webcam.
- Cursor movement with index finger.
- Click, scroll, and volume controls via simple gestures.
- Feature toggle gesture to quickly enable/disable actions.

## How It Works
The app captures frames from your webcam, detects a single hand with MediaPipe, and maps hand landmarks to screen coordinates. Gestures are inferred from which fingers are raised and the relative distances between fingertips.

## Requirements
- Python 3.8+
- A working webcam

### Python Dependencies
Minimal set (based on imports in the code):
- `opencv-python`
- `mediapipe`
- `pyautogui`
- `numpy`

You can install either the full pinned list or the minimal set:

```bash
pip install -r requirements.txt
```

```bash
pip install opencv-python mediapipe pyautogui numpy
```

## Setup
1. Clone the repo and enter it.
2. Install dependencies (see above).
3. Run the app:

```bash
python main.py
```

Press `q` to quit.

## Gestures
The app recognizes the following gestures (right or left hand):

- **Move cursor**: Index finger up.
- **Click**: Thumb + index up, then bring their tips together.
- **Scroll**: Index + middle up, then move the joined fingers horizontally.
- **Toggle features**: Bring all five fingertips together (toggles actions on/off).
- **Volume mode**:
  - **Arm**: Thumb + pinky up.
  - **Adjust**: Pinky only to change volume (right hand = up, left hand = down).

Notes:
- All gestures are defined in `modules/hand_detector.py` and actions in `modules/actions.py`.
- A tracking rectangle is drawn internally. The preview window is currently commented out in `main.py`.

## Troubleshooting
- If the cursor feels jumpy, tweak `smoothness` in `modules/actions.py`.
- Ensure your webcam is not in use by another app.
- On macOS, you may need to grant screen recording/accessibility permissions for PyAutoGUI.

## Project Structure
- `main.py`: App entry point and webcam loop.
- `modules/hand_detector.py`: Hand detection and gesture classification.
- `modules/actions.py`: Cursor, click, scroll, and volume actions.
- `requirements.txt`: Full dependency list.

