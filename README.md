<!-- PROJECT HEADER -->
# 🐾 Two-Stage Species Identification Model 🌲

<!-- BADGES -->
![YOLOv8](https://img.shields.io/badge/Stage_1-YOLOv8_Detection-00A65A)
![ResNet50](https://img.shields.io/badge/Stage_2-ResNet50_Classification-blue)
![PyTorch](https://img.shields.io/badge/Framework-PyTorch-EE4C2C?logo=pytorch)
![Course](https://img.shields.io/badge/Course-COS30049_CTIP-orange)

**COS30049 CTIP Project's AI Core:** An automated two-stage pipeline designed to identify wildlife species from camera trap images. This system bridges the gap between raw field data and structured conservation insights by combining state-of-the-art object detection with fine-tuned deep learning classification.

---

<!-- AI ARCHITECTURE OVERVIEW -->
## 🧠 AI Core Architecture
The model operates in two distinct logical phases to maximize accuracy in complex natural environments:

1.  **Stage 1: Detection (YOLOv8)** – Scans high-resolution camera trap images to detect the presence of animals and generate precise bounding box coordinates.
2.  **Stage 2: Classification (ResNet50)** – Crops the detected animal from the original frame and performs fine-grained classification across **64 different species classes**.

---

<!-- DATA PREPARATION SECTION -->
## 📊 Data Engineering & Augmentation
<!-- Internal Note: This section highlights your data handling skills -->
To ensure model robustness against imbalanced wildlife datasets, a custom preprocessing pipeline was developed:

*   **Balanced Collection:** `data_collection.py` maps similar species names to a standard format and enforces a 50-image-per-species limit to prevent class dominance.
*   **Advanced Augmentation:** `data_augment.py` utilizes the **Albumentations** library to generate synthetic data for underrepresented species using noise injection, rotation, and brightness adjustments.
*   **Roboflow Integration:** Final dataset curation (v10) managed via Roboflow for precise annotation and preprocessing.

---

<!-- STAGE 1 LOGIC -->
## 🔍 Stage 1: Species Detection (YOLOv8)
*   **Task:** Object Detection.
*   **Training:** 25 epochs at 800x800 resolution via `YOLOv8_Species_Detection.ipynb`.
*   **Inference Logic:** Identifies "Animal" vs "Empty" frames to reduce false triggers in the database.
*   **Output:** Bounding box coordinates used as input for the next stage.

---

<!-- STAGE 2 LOGIC -->
## 🦁 Stage 2: Species Classification (ResNet50)
*   **Task:** Multi-class Image Classification.
*   **Architecture:** Fine-tuned **ResNet50** (Pretrained on ImageNet).
*   **Training:** 10 epochs with `ResNet_Species_Classification.ipynb`.
*   **Dynamic Cropping:** `YOLOv8_Cropped_ResNet_Data.ipynb` automates the extraction of animal-only crops to eliminate background noise (foliage/terrain), significantly boosting classification accuracy.

---

<!-- INTEGRATION & PIPELINE SECTION -->
## 🔌 Pipeline Integration & Deployment
<!-- Internal Note: This explains how your AI connects to the rest of the group project -->
The script `TwoStage_Predictions_DB.py` acts as the system orchestrator:
1.  **OCR Metadata Extraction:** Uses **Tesseract OCR** to read date, time, and temperature stamps directly from the camera trap image footer.
2.  **Sequential Inference:** Runs YOLOv8 → Crops Image → Runs ResNet50.
3.  **Database Storage:** Automatically uploads species ID, confidence scores, site location, and environmental metadata to a **MySQL database**.

---

<!-- TECHNICAL DEPENDENCIES -->
## 🛠️ Technical Stack
*   **Detection:** Ultralytics YOLOv8
*   **Classification:** PyTorch (Torchvision)
*   **Data Processing:** OpenCV, Albumentations, NumPy, Pandas
*   **OCR:** Pytesseract
*   **Database:** MySQL Connector

---
