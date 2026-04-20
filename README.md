# Assignment 4 – Optical Flow with Lucas-Kanade Tracking

**Course:** CAP 4453 – Robot Vision  
**Language:** C++  
**Libraries:** OpenCV  
**Topic:** Motion estimation and feature tracking using the Lucas-Kanade algorithm

---

## Overview

This project implements optical flow analysis using the pyramidal Lucas-Kanade method via OpenCV. It supports two modes: tracking motion between two static images, or tracking features in real time through a live camera feed. Corner features are detected using the Shi-Tomasi algorithm, tracked across frames, and visualized as flow vectors drawn over the image.

---

## How It Works

1. **Corner Detection (Shi-Tomasi)**  
   `cvGoodFeaturesToTrack` identifies strong corner features in the first frame — points that are distinct enough to be reliably tracked. `cvFindCornerSubPix` refines their positions to sub-pixel accuracy.

2. **Lucas-Kanade Pyramidal Tracking**  
   `cvCalcOpticalFlowPyrLK` tracks each corner from frame A to frame B using a multi-scale pyramid approach. This makes tracking robust to larger motions that would be missed at a single scale.

3. **Flow Visualization**  
   For each successfully tracked feature:
   - A **blue circle** marks its position in frame A
   - A **green line** shows the direction and magnitude of movement to frame B

---

## Modes

### `frames` — Static Image Pair
Computes optical flow between two still images (`image0.jpg` → `image1.jpg`). Displays the source images and the resulting flow field side by side.

```bash
./Optical_Flow frames
```

### `camera` — Live Camera Feed (default)
Captures from the default webcam and tracks features in real time across consecutive frames. Includes keyboard controls to adjust tracking behavior on the fly.

```bash
./Optical_Flow camera
# or just:
./Optical_Flow
```

**Keyboard controls (camera mode):**

| Key | Action |
|-----|--------|
| `w` | Increase corners tracked (+10, max 400) |
| `s` | Decrease corners tracked (-10, min 10) |
| `d` | Double pyramid window size (max 64) |
| `a` | Halve pyramid window size (min 2) |
| `ESC` | Exit |

---

## Project Structure

```
assignment4_optical_flow/
├── Optical_Flow.cpp   # Entry point — routes to frames or camera mode
├── Tracker.h          # Tracker class interface
├── Tracker.cpp        # Real-time camera tracking implementation
├── between.h          # between_frames interface
├── between.cpp        # Static image pair optical flow implementation
├── cv_pyrlk.cpp       # Standalone Lucas-Kanade reference implementation
├── common.h           # Shared constants (N_CORNERS, WINDOW_SIZE, etc.)
├── stdafx.h / .cpp    # Precompiled header (MSVC)
├── image0.jpg         # Test input image A
└── image1.jpg         # Test input image B
```

---

## Build

This project was developed on Windows with MSVC and OpenCV 4.9.0. To build on Linux/Mac with g++:

```bash
g++ Optical_Flow.cpp Tracker.cpp between.cpp cv_pyrlk.cpp \
    -o optical_flow \
    $(pkg-config --cflags --libs opencv4)
```

---

## Key Concepts Demonstrated

- Shi-Tomasi corner detection for reliable feature selection
- Sub-pixel corner refinement with `cvFindCornerSubPix`
- Pyramidal Lucas-Kanade optical flow for multi-scale motion estimation
- Real-time video capture and frame-by-frame processing with OpenCV
- Flow field visualization using directional vectors
- Object-oriented design with a reusable `Tracker` class
- Dual-mode architecture: static image analysis and live camera tracking