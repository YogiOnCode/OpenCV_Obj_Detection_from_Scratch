# Object Detection & Tracking from Scratch with OpenCV

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-DNN-5C3EE8?logo=opencv&logoColor=white)
![YOLOv4](https://img.shields.io/badge/YOLOv4-Darknet-00FFFF)

A lightweight object detection and tracking pipeline built from first principles. It uses
**YOLOv4** through **OpenCV's DNN module** to detect objects, and a simple **centroid tracker**
(written by hand, without Deep SORT) to give each object a persistent ID across frames.

---

## How it works

1. **Detection** (`object_detection.py`)
   - Loads YOLOv4 weights and config with `cv2.dnn.readNet`.
   - Uses the CUDA backend when it is available.
   - Runs inference at 608×608 with a confidence threshold of 0.5 and an NMS threshold of 0.4.
2. **Tracking** (`object_tracking.py`)
   - Computes the centre point of every bounding box.
   - Matches each centre to the tracked objects from the previous frame by Euclidean distance (< 20 px).
   - Updates IDs that match, removes IDs for objects that disappear, and assigns new IDs to new detections.
   - Draws boxes, centre points and IDs on each frame.

## Getting started

```bash
git clone https://github.com/YogiOnCode/OpenCV_Obj_Detection_from_Scratch.git
cd OpenCV_Obj_Detection_from_Scratch
pip install -r requirements.txt
```

Download the YOLOv4 model files ([Darknet releases](https://github.com/AlexeyAB/darknet/releases)) and place them as follows:

```
dnn_model/
├── yolov4.weights
├── yolov4.cfg
└── classes.txt      # COCO class names
```

Update the paths in `ObjectDetection.__init__` if your folder layout differs, then run:

```bash
python object_tracking.py
```

Press `Esc` to quit.

## Repository structure

```
├── object_detection.py   # YOLOv4 detector wrapper (OpenCV DNN)
├── object_tracking.py    # Centroid tracker + visualization
└── code.py               # Earlier variant of the tracking loop
```

## Limitations and next steps

- **No occlusion handling:** IDs are lost when objects overlap or leave the frame briefly.
- **Distance-only matching:** fast-moving objects can be reassigned to new IDs.
- **Next steps:** add Kalman-filter prediction or Deep SORT for appearance-based re-identification.

## Tech stack

Python · OpenCV (DNN, CUDA backend) · YOLOv4 · NumPy

## License

Released under the [MIT License](LICENSE).

## Author

**Yogeswaran Amsavalli** · [GitHub](https://github.com/YogiOnCode)
