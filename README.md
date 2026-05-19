
# Few-Shot 3D Brain Tissue Segmentation under Memory Constraints using a U-Net Based Approach

A research oriented 3D medical image segmentation project investigating few-shot brain MRI tissue segmentation under severe memory and class imbalance constraints using a modified 3D U-Net framework.

---

# Overview

This project explores volumetric brain MRI segmentation using deep learning under highly constrained conditions:
- extremely limited training data
- severe class imbalance
- restricted GPU memory (16GB VRAM)

The work is based on the **MRBrainS13** dataset and investigates how architectural modifications, training strategies, inference methods and imbalance-aware sampling influence segmentation performance.

The project was inspired by:
- U-Net
- 3D U-Net
- nnU-Net

and focuses heavily on: data centric experimentation-
-optimisation stability
- patch-based volumetric training
- class imbalance analysis
- failure case investigation

The final system improved baseline performance by approximately **26.8%** while remaining computationally feasible on consumer grade hardware.

---

# Key Contributions

- Custom 3D U-Net implementation in PyTorch
- Patch-based volumetric training under VRAM constraints
- Dice + Cross Entropy hybrid loss optimisation
- InstanceNorm3d + LeakyReLU stabilisation
- Gaussian weighted sliding window inference
- Foreground-aware and class-aware patch sampling
- Extensive ablation studies
- Rare class failure analysis under extreme imbalance conditions

---

# Dataset

The project uses the **MRBrainS13** brain MRI segmentation dataset.

The dataset contains:
- 3D T1-weighted MRI volumes
- voxel-wise segmentation masks
- 9 segmentation classes

Due to dataset restrictions, the data is not included in this repository.

Expected dataset structure:

```text
Dataset/
└── Extracted_Data/
    └── TrainingData/
        ├── 1/
        │   ├── T1.nii
        │   └── LabelsForTraining.nii
        ├── 2/
        ├── 3/
        ├── 4/
        └── 5/
```

---

# Motivation: Memory-Constrained 3D Training

Full-volume 3D medical segmentation is computationally expensive due to the cubic scaling of 3D convolutions.

A single MRI volume in the dataset contains approximately:

```text
240 × 240 × 48 voxels
```

Training directly on full volumes exceeded available GPU memory.

To address this:
- patch-based training was adopted
- volumetric patches were extracted dynamically
- batch size was restricted to 1

This enabled practical 3D training within limited VRAM constraints.

---

# Architecture

The baseline model follows a modified **3D U-Net** encoder-decoder architecture.

## Main Components

- 3D convolution blocks
- encoder-decoder structure
- skip connections
- transposed convolution upsampling
- multi-class voxel-wise prediction

## Optimised Configuration

The final best-performing model used:
- InstanceNorm3d
- LeakyReLU activation
- DiceCE loss
- Gaussian weighted sliding-window inference
- patch size: `96 × 96 × 32`

---

# Architecture Diagram



# Training Pipeline

## Preprocessing

- MRI loading using `nibabel`
- z-score normalisation using non-background voxels only
- patch extraction for volumetric training

## Training Strategy

- Leave-One-Out Cross Validation (LOOCV)
- AdamW optimiser
- patch-based training
- Dice + Cross Entropy hybrid loss
- batch size = 1

---

# Experiments and Ablation Studies

Several architectural and optimisation strategies were investigated.

## Architecture & Inference Improvements

| Method | Mean Dice | Class 4 Dice |
|---|---|---|
| Patch96 + DiceCE | 0.803 | 0.259 |
| + InstanceNorm + LeakyReLU | 0.820 | 0.329 |
| + Gaussian Inference | 0.825 | 0.354 |

---

## Class Imbalance Experiments

| Method | Mean Dice | Class 4 Dice |
|---|---|---|
| Baseline Model | 0.557 | - |
| Best Model | 0.825 | 0.354 |
| Foreground-Aware Patching | 0.823 | 0.327 |
| Class-4-Aware Patching | 0.825 | 0.341 |
| Data Augmentation | 0.811 | 0.340 |
| Focal Loss | 0.808 | 0.329 |
| Final Combined System | 0.815 | 0.363 |

---

## Patch Size Ablation

| Patch Size | Mean Dice | GPU Memory |
|---|---|---|
| 64 × 64 × 32 | 0.642 | 345 MB |
| 80 × 80 × 32 | 0.776 | 489 MB |
| 96 × 96 × 32 | 0.803 | 658 MB |

These experiments demonstrate the trade-off between:
- spatial context
- segmentation performance
- computational cost

---

# Class Imbalance Analysis

A major challenge in the dataset was extreme class imbalance.

The most severe case occurred in:
```text
Subject 4 → Class 4
```

which contained only:

```text
4 voxels
```

This caused severe instability in Dice score evaluation and highlighted limitations of overlap-based metrics for extremely sparse anatomical structures.

The project therefore focused not only on improving segmentation accuracy, but also on understanding *why* certain classes failed.

---

# Example Outputs

## MRI Segmentation Example


## Training Curves


## Class Imbalance Analysis


---


---

# Repository Structure

```text
3d-medical-image-segmentation/
│
├── README.md
├── report.pdf
├── requirements.txt
├── segmentation_training.ipynb
│
├── images/
│   ├── architecture.png
│   ├── segmentation_example.png
│   ├── loss_curves.png
│   └── class_imbalance.png
│
└── outputs/
```

---

# Future Improvements

Potential future work includes:
- attention-gated segmentation
- Hausdorff distance evaluation
- nnU-Net style automatic configuration
- improved rare-class localisation
- test-time augmentation
- ensemble-based inference

---

# Author

**Muhammad Ahmad Raza**  
Queen Mary University of London

