# 🧘 Yoga Pose Detection — Sun Salutation Guide

> A real-time computer vision system that detects yoga poses and guides users through all **12 steps of the Sun Salutation (Surya Namaskar)** sequence using angle-based skeletal analysis powered by MediaPipe and OpenCV.

---

## 🎥 Demo

> Place your demo video or GIF here.

```
assets/
└── demo.gif   ← Add your screen recording here
```

---

## ✨ Features

| Feature | Description |
|---|---|
| 📸 Static Image Detection | Analyze a single image to identify a yoga pose with annotated landmarks |
| 🎥 Real-time Pose Detection | Live webcam feed that classifies poses frame-by-frame |
| 🌅 Sun Salutation Sequencer | Guided 12-step Surya Namaskar tracker that advances automatically |
| 📐 Angle-Based Classification | Uses joint angles (elbow, shoulder, hip, knee) for precise pose matching |
| 🦴 Landmark Visualization | Draws all 33 MediaPipe skeleton points and bone connections on screen |
| ✅ Step Completion Feedback | Displays "COMPLETE STEP!" notification when a pose is held correctly |
| 🔄 Round Counter | Tracks the number of completed full Sun Salutation rounds |

---

## 🌅 The 12 Steps of Sun Salutation (Surya Namaskar)

The system guides users through the following sequence in order:

| Step | Pose (Sanskrit) | Description |
|---|---|---|
| 1 | **Tadasana** | Mountain Pose — standing tall, arms at sides |
| 2 | **Urdhva Hastasana** | Raised Arms Pose — arms stretched overhead |
| 3 | **Uttanasana** | Standing Forward Fold — deep forward bend |
| 4 | **Ardha Uttanasana** | Half Forward Fold — flat back, halfway up |
| 5 | **Phalakasana** | Plank Pose — high push-up position |
| 6 | **Chaturanga Dandasana** | Low Plank — elbows bent, body hovering |
| 7 | **Urdhva Mukha Svanasana** | Upward-Facing Dog — chest lifted, hips low |
| 8 | **Adho Mukha Svanasana** | Downward-Facing Dog — inverted V shape |
| 9 | **Ardha Uttanasana** | Half Forward Fold — returning up |
| 10 | **Uttanasana** | Standing Forward Fold — second fold |
| 11 | **Urdhva Hastasana** | Raised Arms Pose — sweeping arms back up |
| 12 | **Tadasana** | Mountain Pose — return to standing |

---

## 🔬 How the Pose Detection Works

The system tracks **8 joint angles** per frame, calculated using the dot product between bone vectors. Each pose is defined by an angle threshold range for all relevant joints.

**Joints tracked:**

```
Left & Right:  Elbow · Shoulder · Hip · Knee
```

**Angle calculation formula:**

```
cos(θ) = (BA · BC) / (|BA| × |BC|)
θ = arccos(cos(θ)) → converted to degrees
```

A pose is confirmed when **all joint angles fall within their defined threshold ranges simultaneously**, making the detection robust against slight body variations.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────┐
│           Webcam / Image            │
└──────────────────┬──────────────────┘
                   │
┌──────────────────▼──────────────────┐
│         MediaPipe Pose Model        │
│   Extracts 33 body landmarks (x,y)  │
└──────────────────┬──────────────────┘
                   │
┌──────────────────▼──────────────────┐
│       Joint Angle Calculator        │
│  8 angles: elbows, shoulders,       │
│  hips, knees (left & right)         │
└──────────────────┬──────────────────┘
                   │
┌──────────────────▼──────────────────┐
│        Pose Classifier              │
│  Rule-based angle threshold checks  │
│  Matches to one of 8 pose classes   │
└──────────────────┬──────────────────┘
                   │
┌──────────────────▼──────────────────┐
│     Sequence Tracker (Mode 3)       │
│  Checks if detected pose matches    │
│  the expected step → advances index │
│  Increments completed_rounds on     │
│  full sequence completion           │
└──────────────────┬──────────────────┘
                   │
┌──────────────────▼──────────────────┐
│         OpenCV UI Overlay           │
│  Current pose · Next pose · Status  │
│  Landmark skeleton drawn on frame   │
└─────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Library | Purpose |
|---|---|
| **Python 3.8+** | Core language |
| **OpenCV** (`cv2`) | Video capture, frame processing, UI overlays |
| **MediaPipe** | Body pose landmark detection (33 skeletal points) |
| **NumPy** | Vector math for joint angle calculation |
| **Matplotlib** | Static image visualization in notebook mode |

---

## 📂 Project Structure

```
yoga-pose-detection/
│
├── Yoga_Code.ipynb         # Main notebook — all 3 detection modes
│
├── testing/
│   └── test6.jpg           # Sample image for static detection (Mode 1)
│
├── assets/
│   └── demo.gif            # Demo recording (add your own)
│
└── README.md               # This file
```

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/yoga-pose-detection.git
cd yoga-pose-detection
```

### 2. Create a Virtual Environment (Recommended)

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install opencv-python mediapipe numpy matplotlib jupyter
```

Or create a `requirements.txt` with the following and run `pip install -r requirements.txt`:

```
opencv-python
mediapipe
numpy
matplotlib
jupyter
```

### 4. Launch the Notebook

```bash
jupyter notebook Yoga_Code.ipynb
```

---

## 📓 Notebook — Three Detection Modes

The notebook is organized into **3 progressive modules**, each building on the last.

---

### Mode 1 — Static Image Analysis

Tests pose detection on a **single image file**. Useful for calibrating angle thresholds before going live.

- **Input**: `testing/test6.jpg` (change the path to your own image)
- **Output**: Annotated image with pose label + printed joint angles in the console
- **Good for**: debugging pose thresholds, verifying landmark accuracy on a still shot

```python
image_path = "testing/test6.jpg"  # ← Change to your image path
```

---

### Mode 2 — Real-time Pose Detection (No Sequence)

Opens the **webcam** and classifies your current pose in real time, with no step-tracking logic.

- Landmarks and the detected pose name are overlaid on the live feed
- Press `q` to quit
- Useful for testing individual poses freely without worrying about sequence order

---

### Mode 3 — Sun Salutation Sequencer ⭐

The **full guided experience**. The system:

1. Shows the **current pose** to perform on screen
2. Detects when your body matches that pose
3. Flashes a **"COMPLETE STEP!"** confirmation
4. Automatically advances to the **next pose** in the sequence
5. Counts and displays **completed full rounds** when all 12 steps finish

- Press `q` to quit at any time

---

## ⚙️ Pose Angle Thresholds Reference

Each pose is validated using threshold ranges across all 8 joint angles (left & right sides must both match):

| Pose | Elbow | Shoulder | Hip | Knee |
|---|---|---|---|---|
| Tadasana | 165–185° | 0–10° | — | 160–185° |
| Urdhva Hastasana | 155–190° | 155–180° | — | 170–185° |
| Uttanasana | 120–190° | 70–150° | 10–50° | 120–190° |
| Ardha Uttanasana | 110–185° | 65–130° | 30–90° | 130–180° |
| Phalakasana | 150–190° | 50–105° | 100–180° | 150–185° |
| Chaturanga Dandasana | ≤ 140° | 0–50° | 150–190° | 150–190° |
| Urdhva Mukha Svanasana | 160–190° | 5–35° | 110–170° | 130–185° |
| Adho Mukha Svanasana | 150–180° | 155–180° | 30–70° | 150–185° |

> **Tip:** If the system isn't detecting a pose, run Mode 1 on a photo of yourself in that pose. The printed angles will tell you exactly which joint is out of range — adjust the threshold accordingly.

---

## 💡 Tips for Best Results

- **Lighting**: Use a well-lit room. MediaPipe performs best with clear contrast between your body and the background.
- **Camera Distance**: Stand 1.5–2.5 metres from the camera so your full body is in frame.
- **Clothing**: Fitted clothing gives more accurate landmark detection than baggy items.
- **Background**: A plain, uncluttered background reduces false detections.
- **Camera Height**: Position the camera at roughly chest height and tilted slightly downward for full-body coverage.

---

## 🔮 Potential Improvements

- **Hold Timer** — require the user to hold a pose for N seconds before the system advances
- **Audio Cues** — text-to-speech announcements for the next pose name
- **Confidence Scoring** — replace hard angle thresholds with a fuzzy scoring system for smoother transitions
- **Streamlit GUI** — build a polished web interface instead of running in a notebook
- **Session Report** — export a summary of poses completed, time taken, and accuracy per step
- **More Sequences** — add support for Vinyasa flows, Warrior sequences, or custom user-defined sequences

---

## 🚢 Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit: Yoga Pose Detection — Sun Salutation"
git remote add origin https://github.com/your-username/yoga-pose-detection.git
git branch -M main
git push -u origin main
```

**Recommended `.gitignore`:**

```gitignore
__pycache__/
*.pyc
.venv/
venv/
.ipynb_checkpoints/
```

---

## 📄 License

This project was developed as part of an internship program.

© 2026 Daniel — Raffles University Internship Project at **DigiMagic**
