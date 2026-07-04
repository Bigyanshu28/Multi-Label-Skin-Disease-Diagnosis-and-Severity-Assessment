# A Clinician-Centered AI System for Multi-Label Skin Disease Diagnosis and Severity Assessment

## 📖 Abstract
Skin disease is common worldwide, but diagnosis can be delayed or inconsistent due to variable clinician experience and image quality. This project develops a clinically-robust AI system that automatically detects and classifies 22 skin disease categories from clinical images, supports multi-label outputs for co-occurring conditions, and provides meaningful severity grading.

By combining careful dataset curation, multi-expert annotation, and modern multi-task modeling, this system is designed to meet the practical needs of clinical deployment: reliability, transparency, and fairness.

## 🎯 Objectives

### Primary Objective
Build a clinically-robust, multi-stage AI system that:
* Performs multi-label classification across 22 skin diseases from clinical images.
* Supports per-disease specialist modules that provide finer-grained outputs (subclass detection, grade classification, severity grading).
* **Success Criteria:** A functioning prototype returning probabilities for 22 diseases alongside interpretability and uncertainty estimates.

### Phase 1: Acne-Specific Module
* **Subclass Detection:** Detect 7 acne morphological subclasses (papule, pustule, cyst, scar, nodule, whitehead, blackhead) with F1 ≥ 0.70.
* **Severity Classification:** Divide images into Non-Inflammatory (Grade 1) and Inflammatory (Grades 2–4).
* **Multi-Task Approach:** Implement a shared encoder with separate heads for subclass detection, grade classification, and severity prediction.

### Phase 2: Rosacea-Specific Module
* **Subtype Classification:** Classify rosacea into clinical subtypes (erythematotelangiectatic, papulopustular, phymatous, ocular) with F1 ≥ 0.70 and macro-AUC ≥ 0.80.
* **Feature Scoring:** Include severity scores for erythema intensity and telangiectasia extent.

---

## 🏗️ Framework & Architecture

To address the complexity of facial acne analysis, the pipeline is organized as a modular, multi-stage deep learning framework. The system mirrors a dermatologist’s diagnostic workflow, moving sequentially from detection to classification, validation, and severity grading.

### Core Pipeline Components
* **M1 (Acne Presence Detection):** Binary classification to determine if acne is present. Acts as an initial filter.
* **M2 (Acne Type Classification):** Multilabel classification identifying whether detected acne is inflammatory or non-inflammatory.
* **M3 (Acne Subtype Detection):** Multiclass classification of structural morphology (Blackheads, Whiteheads, Papules, Pustules, Nodules, Cysts, Scars).
* **M4 (Object Detection Validation):** Localizes acne lesions on the face via bounding box prediction to spatially confirm lesion presence.
* **Severity Grading Model:** Post-processing classification to determine the final clinical grade.

### Hybrid Input Interface
The system accepts both visual and clinical context to generate a comprehensive report:
* **Image Data:** 70% input weight.
* **Patient Text Input:** 30% input weight (via Clinical Questionnaire).

---

## 📋 Clinical Questionnaire & Severity Grading

We developed a 17-item patient/clinical questionnaire to capture acne morphology, distribution, course, and psychosocial impact. Each response is assigned a numeric weight, aggregating into a total score (0–60). 

This continuous score is mapped to ordinal grade categories reflecting clinical severity:

| Grade | Total Weighted Score | Clinical Category |
|---|---|---|
| **Grade 1** | 0 – 10 | Non-inflammatory acne |
| **Grade 2** | 11 – 30 | Inflammatory acne |
| **Grade 3** | 31 – 45 | Inflammatory acne |
| **Grade 4** | 46 – 60 | Inflammatory acne |

*Note: The weighted score is used as a continuous target for regression, while the Grade label (1-4) is used for the ordinal classification head.*

---

## 📂 Dataset & Preprocessing

**Data Sources:**
* **Private Dermatology Firm:** Unannotated images obtained under a research collaboration agreement.
* **Open-Source Platforms:** Diverse clinical imagery to increase variance in lesion types, skin tones, and lighting conditions.

**Preprocessing Pipeline:**
Raw images undergo a robust augmentation workflow before entering the model:
1. **Noise Removal:** Gaussian denoising filters applied to improve lesion clarity.
2. **Image Resizing:** Standardized to 640x640 pixels.
3. **Quality Filtering:** Removal of blurry or underexposed data.
4. **Normalization:** Pixel values normalized for training stability.
5. **Data Augmentation (6x Expansion):** Random cropping, color jittering (brightness/contrast/saturation), rotations (±15° and ±30°), horizontal flipping, and elastic deformation.

---

## 🏷️ Annotation Pipeline

Annotations were conducted under the supervision of Dr. Rupinder Kaur using a semi-automated, human-in-the-loop approach.

1. **Bounding Box Generation :** Generates coarse region proposals around potential acne lesions.
2. **Acne-Type Labeling :** Seven independent binary classifiers assign specific morphological subtypes to the bounding boxes. Operates independently to allow for multilabel co-occurrence.
3. **Annotation Verification:** A feature-level ensemble verifies subtype labels against visual evidence. The ConvNet captures local textures (pores, inflammation), while the Swin Transformer captures global spatial relationships.

---

## 🚀 Impact & Expected Outcomes
By providing multi-label disease detection, calibrated severity assessments, and clear interpretability outputs (Grad-CAM), this AI prototype is designed for direct clinical integration. It aims to reduce diagnostic variability, accelerate appropriate care routing, and ultimately improve patient outcomes in settings with limited dermatology expertise.
