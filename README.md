# Star Wars Chip Classifier & Counterfeit Detector 🎰

**The Capstone Project**

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

## 📂 Project Structure
* `capture.py`: Script for collecting raw training data via webcam.
* `Final_Group_5.ipynb`: The main notebook containing the preprocessing, training, and final scanning logic.
* `credit_classifier.h5`: The trained CNN model file.
* `label_classes.npy`: The label encoder for chip values.

---

## 🔬 Project Pipeline

### Phase 1: Data Collection (Capture)
We utilized a custom script (`capture.py`) to build a balanced dataset of 2,800 images:
* **Yellow Chips:** 300 samples (High Value)
* **Blue Chips:** 200 samples (Medium Value)
* **Red Chips:** 150 samples (Low Value)

### Phase 2 & 3: Preprocessing ("Double-Trap")
*Solving the "Green Background" Problem*
Standard background subtraction failed due to reflections. We developed a **"Double-Trap" Strategy**:
* **Trap A (Darkness Mask):** Captures dark plastic chips (Red/Blue) using Value thresholds.
* **Trap B (Color Mask):** Captures bright Gold/Yellow chips using Hue thresholds.
* **Orientation Normalization:** A pixel-density check flips images 180° so the dot pattern always starts on the left.

### Phase 4: Calibration (Ground Truth)
*Building the Physical Security Gate*
We algorithmically measured every valid chip to establish a "Reference Dictionary" of valid dimensions (Min/Max Length & Thickness). This is used later to filter counterfeits.

### Phase 6: Model Training
* **Input:** 128x64 Grayscale images (Preserves 2:1 aspect ratio).
* **Architecture:** CNN with `ReLU` activation for non-linear pattern recognition.
* **Validation:** Used an 80/20 Train/Test split to prevent overfitting.

### Phase 9: Stress Test (Counterfeit Detection)
The final scanning engine integrates the CNN with Logic Gates. It processes batch images, calculates total value, and marks invalid chips with **RED BOXES**.

---

## 📊 Results & Performance

### 1. Model Accuracy
The CNN model showed excellent convergence:
* **Accuracy:** Reached **>98% accuracy** on the validation set within 15 epochs.
* **Generalization:** Loss curves indicated no overfitting, proving the model learned the "dot patterns" rather than memorizing pixels.

### 2. Counterfeit Detection Success
The "Dual-Gate" system achieved a 100% detection rate on the Fake Dataset:
* **Physical Gate:** Flagged undersized fakes (`⚠️ FAKE Detected: Too Short <120px`).
* **Logic Gate:** Flagged impossible combinations (`❌ Logic Mismatch: Blue cannot be 9360`).

---

## ⚙️ How to Run

### Step 1: Install Requirements
Install the necessary dependencies to run the scripts.
```bash
pip install opencv-python tensorflow numpy pandas matplotlib scikit-learn
```

### Step 2: Capture Data (Optional)
*Note: This step runs locally with a webcam. It is only required if you want to regenerate the dataset.*
Run the capture script with the desired label and color arguments.
```bash
# Example: Capturing 300 images of a Yellow 9360 chip
python capture.py --label 9360 --color yellow
```

### Step 3: Train the Model
*Note: This step runs in the Jupyter Notebook (Google Colab).*
1. Open `Final_Group_5.ipynb`.
2. Ensure the `raw_dataset` path in Phase 1 points to your uploaded images.
3. Run **Phase 1 through Phase 6**.
4. This process will train the CNN and save two critical files:
   * `credit_classifier.h5` (The AI Brain)
   * `label_classes.npy` (The Label Decoder)

### Step 4: Run the Final Test (Scanning Engine)
*Note: This runs the "Grand Master" engine to process batch images.*
1. In the notebook, scroll down to **Phase 9**.
2. Set the `TEST_FOLDER` variable to the path of your test images (e.g., `/content/drive/MyDrive/test_dataset`).
3. Run the cell.
4. The script will apply both Security Gates and the AI Model.
5. **Results:** Annotated images with Green (Valid) or Red (Fake) bounding boxes will be saved to the `testResults` folder.

---
*Created by M*
