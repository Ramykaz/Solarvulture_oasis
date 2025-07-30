# ☀️ SolarVulture IR Project - Infrared Solar Panel Fault Detection 🔍

Welcome to the **SolarVulture IR** project! This repository focuses on **infrared-based fault detection in solar panels** using YOLOv8 object detection and classification models.

## 📁 Project Structure

```
src/
├── Models/               # 📦 Pretrained models for detection & classification
├── Train/                # 🏋️‍♂️ Training scripts for various models
├── Scripts/              # 🧠 Full pipeline scripts for end-to-end inference
├── Data/                 # 🖼️ Dataset (20k images + JSON metadata)
└── README.md             # 📘 Project documentation
```

---

## 🤖 Models

### `Models/`
This folder contains all the trained YOLO models for different stages of the pipeline:

| Model Name                 | Description                                                                                 |
|---------------------------|---------------------------------------------------------------------------------------------|
| `cell_detector.pt`        | Detects solar **cells** from full panel IR images (no defect classification) 🔳            |
| `binary_defect.pt`        | Detects whether each detected cell is **Defective 🟥 or Non-defective 🟩**                 |
| `anomaly_detector.pt`     | Detects and classifies **5 anomaly types**, but underperformed due to class imbalance ❌    |
| `fault_classifier.pt`     | YOLOv8 **image classification model** to classify each cell into **10 fault types** + Normal ✅ |

> ⚠️ `anomaly_detector.pt` has limited utility due to skewed data distribution (majority hotspots, others < 20 samples/class)

---

## 📂 Data

### `Data/`
- Contains a curated dataset of **20,000 IR cell images**.
- Organized for classification of **11 classes** (10 types of faults + 1 No-Anomaly).
- Metadata is stored in `module_metadata.json`:
```json
{
  "image_filepath": "images/13637.jpg",
  "anomaly_class": "Hot-Spot"
}
```

---

## 🏋️‍♂️ Training Scripts

### `Train/`
Scripts to train YOLOv8 models for detection and classification:
- `Binary_defect.ipynb`: Trains binary (defect vs non-defect) model.
- `Cell_detector.ipynb`: Trains YOLOv8 object detector for extracting cells.
- `Anomaly_detection1.ipynb`: Trains the 5-class anomaly detector.
- `fault_classifier.ipynb`: Trains an 11-class image classifier using 224x224 crops of cells.

---

## 🧠 Full Inference Pipeline

### `Scripts/Full_pipeline.ipynb`
The full pipeline does the following:
1. 🖼️ Takes in a panel image (grayscale or color).
2. 🔳 Detects all solar cells using `cell_detector.pt`.
3. 🧪 Passes each detected cell through `binary_defect.pt`.
4. 📊 Sends all crops to `fault_classifier.pt` for specific fault labeling.
5. 🎨 Annotates the original image with bounding boxes and **fault labels** (color-coded).
6. 💾 Saves and optionally displays the final annotated image.

> 🌪️ Handles slanted panel images and image variations with preprocessing & grayscale conversion.

---

## 📌 Notes
- Future improvements:
  - Better panel alignment via geometric correction
  - Augmentation to improve classifier generalization
  - Increase samples for underrepresented fault types

---

## 🧪 Example Use Case
- Upload an IR image from a drone flight
- Run full pipeline:
  - ✅ Cells detected and extracted
  - ❌ Binary classifier filters defective cells
  - 🔍 Classifier identifies **specific fault types**
- Output: Panel image with labeled and colored boxes per cell

---

## 🛠️ Technologies Used

- [Ultralytics YOLOv8](https://docs.ultralytics.com)
- Roboflow dataset management
- Python, OpenCV, Matplotlib
- Google Colab for training

---

## 🌞 Project Goal

> Enable accurate, scalable detection of **PV cell-level faults** using aerial IR imagery, supporting maintenance, monitoring, and **sustainable energy reliability**.

---

## 🙌 Credits
Project maintained by the **SolarVulture Team** 🌍  
Dataset: Internal + Open sources  
Models trained & optimized with 🤖 Ultralytics + Roboflow

---