# 3D_medical_image_segmentation


A 3D U-Net based brain MRI segmentation project using the MRBrainS13 dataset, developed under limited data and memory constraints.

## Overview

This project focuses on multi-class 3D medical image segmentation for brain MRI scans. The aim was to segment different brain tissue classes using a custom 3D U-Net architecture and evaluate performance under a small-data setting.

The project includes:

- 3D MRI preprocessing
- 
- Patch-based training
- 
- 3D U-Net implementation in PyTorch
- 
- Dice-based loss functions
- 
- Leave-One-Out Cross Validation
- 
- Class imbalance analysis
- 
- Failure case investigation

The final system improved baseline performance by approximately **26.8%** through architectural and training improvements inspired by U-Net, 3D U-Net, and nnU-Net.

---

## Dataset

The project uses the **MRBrainS13** dataset, containing brain MRI volumes and corresponding segmentation labels.

Due to dataset size and access restrictions, the dataset is not included in this repository.

