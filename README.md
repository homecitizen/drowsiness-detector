# Eye Closure Detection with EAR

An educational Python and Jupyter project that explores facial landmarks, the eye aspect ratio (EAR), and a time-based warning for prolonged eye closure. The notebook includes calibration, plots, and a webcam prototype with visual and audio alerts.

This is an experimental prototype, not a validated drowsiness or medical detector. Lighting, head pose, glasses, occlusion, missed detections, processing delays, and individual differences can affect results. Do not use it to make driving or safety decisions.

## Requirements

- macOS with Apple Silicon, a webcam, and camera permission for VS Code or the application running Python.
- Python 3.11, VS Code with the Python and Jupyter extensions.
- Internet access for installing packages and downloading the model.

The sound uses macOS `afplay` and a built-in system sound. OpenCV's GUI may behave differently in Jupyter; focus its window and press `q` to exit. If the window does not respond, interrupt the kernel. Restarting the kernel clears variables in memory.

## Setup

```bash
python3.11 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
mkdir -p models
curl --fail --location --output models/face_landmarker.task https://storage.googleapis.com/mediapipe-models/face_landmarker/face_landmarker/float16/1/face_landmarker.task
```

Clone or download this repository and run the commands above from its root. Open `detector_github.ipynb` in VS Code and select `.venv` as its notebook kernel. The model file is downloaded separately and is not included in this repository. MediaPipe's model and dependencies have their own licenses.

## Notebook workflow

1. Run the first cells with your face visible to capture a frame, detect landmarks, and calculate EAR for each eye.
2. Run the calibration cell. When prompted, press Enter for the open-eye batch, then again for the closed-eye batch. Stay seated and face the camera. The next cell plots both batches and chooses a threshold halfway between the 90th percentile of closed-eye samples and the 10th percentile of open-eye samples. If calibration fails, check the lighting and repeat both batches.
3. Record a separate 10-second sequence to inspect EAR and closure durations. Try normal blinks and a short voluntary closure to compare their durations.
4. Run the final cell. When the measured EAR remains below the threshold for one second, it displays a warning and plays a sound at the start of the alert. Press `q` with the camera window focused to stop.

Before committing notebook changes, clear its saved outputs in VS Code and save it. The capture cells can store webcam images inside the notebook file.

## Method and limitations

EAR relates vertical eye opening to eye width using six landmarks per eye. The project averages both eyes and compares the result with a personally calibrated threshold. A temporal condition distinguishes a sustained low EAR from brief dips. EAR is a geometric proxy for eye opening, not an exact FACS AU43 annotation; a prolonged closure alone does not establish sleepiness or pain. A missing face resets the live closure timer.

The plots describe an individual recording. Measuring accuracy across people and conditions would require separate test recordings and reliable labels.

## References

- Soukupová and Čech, [Real-Time Eye Blink Detection using Facial Landmarks](https://vision.fe.uni-lj.si/cvww2016/proceedings/papers/05.pdf), 2016.
- [MediaPipe Face Landmarker documentation](https://developers.google.com/edge/mediapipe/solutions/vision/face_landmarker?tab=Python).
- [MediaPipe Python setup](https://developers.google.com/edge/mediapipe/solutions/setup_python).
