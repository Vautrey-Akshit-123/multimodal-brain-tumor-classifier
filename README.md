# Brain Tumor Classification Using Machine Learning and Deep Learning (MRI, CT & PET)

## 📌 Overview
This project presents a systematic comparison of **Machine Learning (ML)** and **Deep Learning (DL)** techniques for **brain tumor classification** across three major neuroimaging modalities — **MRI, CT, and PET**. The goal is to evaluate how classical ML (SVM) and modern DL (CNN via transfer learning) approaches perform individually and in a hybrid setup, and to identify performance patterns that are specific to each imaging modality.

The best-performing configuration was a **hybrid ResNet18 (feature extractor) + SVM classifier**, which consistently outperformed the handcrafted-feature SVM baseline across all three modalities.

An interactive **Streamlit web application** was also built to demonstrate real-time tumor classification from uploaded brain scan images.

---

## 🎯 Objectives
1. Investigate and preprocess MRI, CT, and PET brain images using a consistent, standardized pipeline.
2. Implement and evaluate classical ML (SVM) and deep learning (CNN/ResNet18) methods for tumor detection and classification.
3. Benchmark the models using accuracy, precision, recall, F1-score, and ROC-AUC.
4. Identify modality-specific performance patterns and provide recommendations for clinical workflow integration.

---

## 🧠 Datasets
Publicly available datasets sourced from **Kaggle**:

| Modality | Source | Classes | Total Images |
|----------|--------|---------|---------------|
| MRI | Brain MRI Images for Brain Tumour Detection | Glioma, Meningioma, Pituitary, No Tumor | 4,413 |
| CT | Brain CT Images Dataset | Tumor / Healthy | 4,618 |
| PET | Brain PET Scans Dataset | Tumor / Healthy | 81 |

> Note: The three datasets are **independent and unpaired** (different patient populations, not co-registered), reflecting a realistic constraint of multi-modal medical imaging research.

---

## ⚙️ Methodology
1. **Data Preprocessing**: Image resizing (224×224), normalization/standardization, and augmentation (rotation, flipping, zoom, brightness/contrast adjustment).
2. **Feature Engineering**:
   - **Handcrafted features**: GLCM texture features, Hu moments, intensity histograms (37-dim).
   - **Deep features**: ResNet18 penultimate-layer embeddings (512-dim), pretrained on ImageNet.
3. **Models**:
   - Classical ML: **SVM with RBF kernel**, hyperparameter-tuned via grid search + cross-validation.
   - Deep Learning: **CNN (ResNet18)** using transfer learning (feature extraction + optional fine-tuning).
   - Hybrid: **ResNet18 deep features fed into an SVM classifier**.
4. **Evaluation**: Accuracy, Precision, Recall, F1-score (macro), ROC-AUC, and Confusion Matrices — computed separately per modality and per task (binary/multiclass).
5. **Deployment**: A Streamlit-based web app (`app.py`) for interactive, real-time predictions on uploaded scans.

---

## 📊 Key Findings
- The **ResNet18 + SVM hybrid** consistently outperformed the handcrafted-feature SVM baseline across all three modalities (MRI, CT, PET), for both binary and multiclass classification tasks.
- **MRI and PET** showed the strongest separability between tumor and healthy classes, while **CT multiclass classification** was comparatively more challenging due to lower soft-tissue contrast and class overlap.
- The **PET dataset was very small** (81 images total), so results on that modality should be interpreted cautiously — near-perfect scores on a small test set are more likely a sign of limited data diversity than of true generalizable performance, and are flagged as a limitation rather than a benchmark result.
- Detailed metrics (accuracy, precision, recall, F1-score, ROC-AUC) and confusion matrices for every modality/task are available in the `results/` folder and in the full dissertation report, but are intentionally not reproduced here since headline accuracy numbers can be misleading outside the context of dataset size, class balance, and validation strategy.

---

## 🖥️ Web Application
A lightweight **Streamlit** app allows users to:
- Upload an MRI, CT, or PET brain scan image
- Select the imaging modality and classification task (binary/multiclass)
- Get an instant prediction with a confidence score and probability distribution

---

## 📁 Project Structure
```
Brain_Tumor_Project/
│
├── Step1_Dataset_Inspection_and_Preparation.ipynb   # Dataset review, quality check, class balance
├── Step2_Preprocessing_and_Datasets.ipynb           # Resizing, normalization, augmentation, splitting
├── Step3_Feature_Extraction.ipynb                   # Handcrafted + ResNet18 deep feature extraction
├── Step4_Model_Training.ipynb                       # SVM & CNN training, hyperparameter tuning
├── Step5_Final_Evaluation.ipynb                     # Metrics, confusion matrices, ROC curves
├── CT_images.ipynb                                  # CT-specific preprocessing/experiments
├── PET_images.ipynb                                 # PET-specific preprocessing/experiments
├── app.py                                           # Streamlit web application
│
├── mri/                                             # MRI dataset (Healthy / Tumor subfolders)
├── ct/                                               # CT dataset (Healthy / Tumor subfolders)
├── pet/                                              # PET dataset (Healthy / Tumor subfolders)
│
├── splits/                                          # Train/Val/Test splits per modality
├── features/                                        # Extracted handcrafted & deep feature files (.npy)
├── results/                                          # Output metrics (CSV) and figures (PNG)
├── models/                                           # Saved trained model files
│
├── class_names.json                                 # Class label mappings
├── dataset_summary_step1.xlsx                       # Dataset inspection summary
└── dissertation.docx                                # Full written dissertation report
```

---

## 🛠️ Tech Stack
- **Language**: Python 3.13
- **Deep Learning**: PyTorch, Torchvision (ResNet18 transfer learning)
- **Machine Learning**: Scikit-learn (SVM, GridSearchCV, metrics)
- **Image Processing**: OpenCV, Scikit-image (GLCM texture features)
- **Data Handling**: Pandas, NumPy
- **Visualization**: Matplotlib, Seaborn
- **Web App**: Streamlit

---

## ⚠️ Limitations
- Datasets are unpaired/independent across modalities (no true multi-modal fusion at the patient level).
- Binary classification only for CT/PET (no tumor subtype grading for these modalities).
- Public, curated datasets may not fully reflect real-world clinical variability (scanner differences, artifacts, noise).
- No real-time or adversarial robustness testing was performed.

---

## 🔮 Future Work
- Multi-grade/severity tumor classification (not just presence/absence)
- Real-time and adaptive CAD systems with concept-drift handling
- Exploration of Vision Transformers, 3D CNNs, and ensemble methods
- Adversarial robustness testing
- Integration with hospital PACS/RIS systems
- Use of larger, more diverse, real-world clinical datasets

---

## 📖 Citation / Author
**Author**: Akshit Vautrey
**Contact**: vautreyakshit@gmail.com

This project was developed as part of an academic dissertation on the application of AI in medical image analysis for neuro-oncology.

---

## ⚖️ Ethical Note
This project uses only publicly available, anonymized datasets. No personally identifiable patient information was used. The trained models are a **research proof-of-concept** and are **not intended for clinical diagnostic use** without further validation, regulatory approval, and radiologist oversight.
