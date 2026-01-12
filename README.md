# Casino Chip Classifier & Counterfeit Detector 🎰

**Capstone Project**

## 📌 Project Overview
This project is a robust Computer Vision and Deep Learning system designed to identify, classify, and authenticate casino chips in real-time. Unlike simple color counters, this system reads the actual dot patterns on the chips using a Convolutional Neural Network (CNN) to determine their specific credit value (e.g., 9360, 756, 25).

Crucially, it features a **"Dual-Gate Security System"** that automatically detects and flags counterfeit chips based on physical dimension anomalies and logical inconsistencies.

## 🚀 Key Features
* **Deep Learning Classification:** Uses a custom CNN trained on 2,800 images to read dot patterns with high precision.
* **"Double-Trap" Detection Logic:** A specialized segmentation algorithm that solves the "Green Background" problem, successfully isolating both dark plastic chips (Red/Blue) and bright reflective chips (Gold/Yellow).
* **Counterfeit Detection (The 2 Gates):**
    1.  **Physical Gate:** Rejects chips that do not match the strict length/thickness ground truth measured during calibration.
    2.  **Logic Gate:** Cross-references the predicted value with the detected color to prevent "hallucinations" (e.g., flagging a Blue chip reading "9360" as FAKE).
* **Automated Dataset Generation:** Custom tools to capture, label, and preprocess data rapidly.

## 🛠️ Tech Stack
* **Language:** Python 3.x
* **Computer Vision:** OpenCV (`cv2`)
* **Deep Learning:** TensorFlow / Keras
* **Data Processing:** NumPy, Pandas, Scikit-Learn
* **Visualization:** Matplotlib

## 📂 Project Pipeline
The project follows a rigorous 9-Phase workflow:

### 1. Data Collection
Automated capture script (`capture.py`) that balanced the dataset:
* Yellow: 300 samples (High Value)
* Blue: 200 samples (Medium Value)
* Red: 150 samples (Low Value)

### 2-3. Preprocessing ("Double-Trap")
Converted images to HSV color space. Used a dual-masking strategy:
* *Trap A:* Dark Value mask for Red/Blue chips.
* *Trap B:* Specific Orange-Hue mask for bright Yellow chips.
* Combined with morphological operations to remove noise from the green table background.

### 4. Calibration (Ground Truth)
Algorithmic measurement of every valid chip to establish the "Reference Dictionary" for physical dimensions (Min/Max Length & Thickness).

### 6. Model Training
* **Input:** 128x64 Grayscale images (Preserves 2:1 aspect ratio).
* **Architecture:** Convolutional Neural Network (CNN) with `ReLU` activation.
* **Performance:** Validated using an 80/20 Train/Test split to prevent overfitting.

### 7-9. Final Scanning Engine
The deployment script that integrates the CNN with the Logic Gates. It processes batch images, draws bounding boxes, calculates the total value of valid chips, and marks invalid chips with a **RED BOX** and **FAKE** label.

## ⚙️ How to Run

### 1. Requirements
Install the necessary dependencies:
```bash
pip install opencv-python tensorflow numpy pandas matplotlib scikit-learn
