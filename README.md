# Hybrid CNN–Transformer Networks with Clinical Metadata for Skin Lesion Classification

## Overview

This research investigates metadata-aware deep learning architectures for automated multi-class skin lesion classification using dermoscopic images from the **ISIC-2019 dataset**.

The study addresses three major challenges in automated skin lesion classification:

- **Class imbalance:** Significant differences in the number of samples across eight skin lesion categories.
- **Computational efficiency:** The need for accurate models with manageable parameter counts and computational requirements.
- **Clinical metadata integration:** Effectively incorporating patient information, including age, sex, and anatomical site, while handling missing values.

To address these challenges, we propose two CNN–Transformer hybrid architectures with different computational capacities: a lightweight model for deployment-oriented applications and a higher-capacity model for improved classification performance.

## Research Objectives

1. Develop a lightweight CNN–Transformer architecture with metadata cross-attention under a 6M-parameter and 1-GMAC computational budget.
2. Design a higher-capacity dual-backbone architecture using attention-based feature fusion and a Kolmogorov–Arnold Network (KAN) classifier.
3. Evaluate both architectures against five baseline models under a consistent training and evaluation protocol.
4. Investigate the contributions of CNN features, Transformer representations, clinical metadata, and classifier design through ablation studies.

## Dataset

**ISIC-2019 Skin Lesion Classification Dataset**

- Original images: 25,331 dermoscopic images.
- Number of classes: 8.
- Original sources: HAM10000, BCN20000, and MSK.
- Metadata: Patient age, sex, anatomical site, and lesion identifier.

### Classification Categories

| Code | Skin Lesion Category |
|---|---|
| MEL | Melanoma |
| NV | Melanocytic Nevus |
| BCC | Basal Cell Carcinoma |
| AK | Actinic Keratosis |
| BKL | Benign Keratosis |
| DF | Dermatofibroma |
| VASC | Vascular Lesion |
| SCC | Squamous Cell Carcinoma |

### Preprocessing

- Near-duplicate image removal using 64-bit perceptual hashing.
- Removed 1,263 near-duplicate images, retaining 24,068 unique samples.
- Stratified 70/10/20 train-validation-test split.
- Grouping by lesion identifier to prevent the same lesion from crossing dataset splits.
- Shades-of-Gray colour constancy correction.
- Image resizing and centre cropping to 224 × 224 pixels.
- Missing clinical metadata handled through learned missing-value embeddings rather than conventional imputation.

## Proposed Methodology

### 1. Lightweight Hybrid CNN–Transformer

The lightweight architecture combines convolutional feature extraction with Transformer-based global representation learning.

**Architecture components:**
- MobileNetV2 backbone for efficient visual feature extraction.
- Compact Vision Transformer with six Transformer blocks and four attention heads.
- Metadata cross-attention mechanism integrating age, sex, and anatomical site.
- Linear classification head for eight-class prediction.

**Performance:**
- Parameters: 3.98M
- Computational cost: 0.631 GMACs
- Balanced Multiclass Accuracy: 60.81%

This architecture provides a balance between classification performance and computational efficiency.

### 2. DEKAN – Dual-Backbone CNN–Transformer Architecture

DEKAN is the higher-capacity architecture designed to improve classification performance through dual-backbone feature extraction and attention-based fusion.

**Architecture components:**
- DenseNet-121 and EfficientNet-B0 for complementary visual feature extraction.
- Learned attention-based feature fusion.
- TinyViT-style Transformer trunk with eight blocks and eight attention heads.
- Metadata cross-attention for clinical information integration.
- Kolmogorov–Arnold Network (KAN) classification head.

**Performance:**
- Parameters: 16.45M
- Computational cost: 6.633 GMACs
- Balanced Multiclass Accuracy: 64.38%

DEKAN achieved the highest BMA among the two proposed main architectures.

## Training Strategy

Both proposed architectures and baseline models were trained using a consistent experimental framework.

**Training techniques:**
- Class-Balanced Focal Loss.
- AdamW optimizer.
- Automatic Mixed Precision (AMP).
- Weighted sampling.
- Gradient clipping.
- Cosine learning rate scheduling.
- Random resized cropping and horizontal/vertical flipping.
- Colour jitter and RandAugment.
- Mixup augmentation.
- Exponential Moving Average (EMA).
- Eight-view Test-Time Augmentation (TTA).

**Evaluation metrics:**
- Balanced Multiclass Accuracy (BMA).
- Macro F1-score.
- Macro AUC.
- Overall accuracy.
- Per-class recall.
- Normalized confusion matrix.

## Experimental Results

### Comparison with Baseline Models

The proposed architectures were evaluated against five baseline models on the held-out ISIC-2019 test set.

| Model | Parameters | BMA |
|---|---:|---:|
| ResNet-18 | 11.69M | 0.3368 |
| MobileNetV2 | 3.50M | 0.4738 |
| EfficientNet-B0 | 5.29M | 0.4946 |
| MobileViT-S | 5.60M | 0.6020 |
| EfficientFormer-L1 | 12.27M | 0.5519 |
| **Lightweight Hybrid** | **3.98M** | **0.6081** |
| **DEKAN** | **16.45M** | **0.6438** |

### Key Experimental Findings

- The lightweight hybrid achieved the strongest BMA among the evaluated models under the 6M-parameter and 1-GMAC budget.
- DEKAN achieved the highest BMA among the two proposed main architectures.
- The lightweight hybrid demonstrated better BMA per million parameters.
- DEKAN achieved stronger performance in several challenging lesion categories, including BCC, VASC, and SCC.

## Ablation Studies

Ablation experiments were conducted to investigate the contributions of different architectural components.

**1. CNN and Transformer Contributions**

The lightweight model's CNN-only and ViT-only variants achieved approximately 0.49 BMA, while combining both components improved performance to approximately 0.608 BMA.

**2. Clinical Metadata Integration**

The DEKAN linear-head variant improved from 0.6302 BMA without metadata to 0.6510 BMA with metadata.

**3. Classifier Comparison**

The DEKAN linear-head configuration achieved 0.6510 BMA, outperforming the KAN-head configuration, which achieved 0.6438 BMA.

These findings demonstrate that architectural complexity alone does not guarantee better classification performance.

## Limitations and Future Work

The study has several limitations:

- Experiments were conducted using a single random seed.
- Two single-backbone DEKAN ablations were not completed.
- External-dataset generalization has not yet been evaluated.
- Further investigation is needed into model latency, calibration, and interpretability on clinical hardware.

Future work will focus on multi-seed experiments, external validation, additional architectural ablations, and deployment-oriented evaluation.

## Technologies and Tools

- **Programming:** Python
- **Deep Learning:** PyTorch
- **Computer Vision:** CNN, Vision Transformer (ViT), TinyViT
- **Architectures:** MobileNetV2, DenseNet-121, EfficientNet-B0
- **Advanced Methods:** Cross-Attention, Feature Fusion, Kolmogorov–Arnold Networks (KAN)
- **Training:** AdamW, Class-Balanced Focal Loss, AMP, EMA, Mixup, TTA
- **Dataset:** ISIC-2019

## Authors

1. Nazif Bin Morshed
2. Md. Samin Yaser
3. Sayem Afridi
4. Mos. Mahabuba Akter Munia
5. Nazmus Shakib

**Department:** Computer Science and Engineering

**Institution:** BRAC University, Dhaka, Bangladesh

**Research Supervisor:** Dr. Md. Ashraful Alam

## Research Paper

The complete research paper is available in IEEE conference paper format.

**Title:** Hybrid CNN–Transformer Networks with Clinical Metadata for Skin Lesion Classification

---

*This repository documents the research, proposed architectures, experimental methodology, and findings of our undergraduate research project at BRAC University.*
