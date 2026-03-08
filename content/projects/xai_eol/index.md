---
title: "Explainable AI for End-of-Line Inspection"
date: 2026-03-07
summary: "Industrial defect detection using YOLOv8 and XAI (LRP, Grad-CAM, LIME) to ensure transparency in manufacturing."
tags: ["Vision"]
tech_stack:
  - Python
  - PyTorch
  - YOLOv8
  - Captum
  - OpenCV
  - Jupyter
links:
  - type: github
    url: https://github.com/gitesh-kumar/XAI-YOLOv8-Vision
    label: Code
featured: true
status: "Completed"
role: "AI Engineer"
highlights:
  - "Validated YOLOv8 detections with 3 XAI frameworks"
  - "Visualized features for 'Loose' vs 'Missing' screws"
  - "Implemented pixel-wise relevance (LRP)"
---

An advanced Computer Vision system designed to solve the 'Black Box' problem in automated manufacturing lines.

## Overview

In safety-critical production environments, it is not enough for an AI to flag a defect; quality engineers need to know *why* a part was rejected. This project implements state-of-the-art Explainable AI (XAI) techniques to provide heatmaps and feature-importance visualizations for an automated screw-inspection assembly.

## 📊 XAI Visualization Methods

### 1. Layer-wise Relevance Propagation (LRP)
LRP identifies which pixels contributed most to a specific class prediction.

![LRP Heatmap Analysis](/uploads/LRP4.png)
*Figure 1: LRP heatmap showing high relevance (red) on screw threads for the 'Fully Tight' detection.*

![LRP Case Study](/uploads/LRP6.png)
*Figure 2: LRP explanation showing how the absence of features triggers a 'Missing Screw' classification.*

### 2. Grad-CAM Activation
Grad-CAM visualizes the "attention" of the final convolutional layers.

![Grad-CAM Results](/uploads/fc7a5e68-54_10_1-res.png)
*Figure 3: Heatmap showing the model's focus on the central assembly area.*

![Grad-CAM Detection](/uploads/grad_cam_loose_tight.png)
*Figure 4: Activation map ensuring the model isn't being distracted by background industrial textures.*

### 3. LIME Explanations
LIME highlights super-pixels that most heavily influenced the YOLOv8 prediction.

![LIME Tight](/uploads/Lime_exp1.png)
*Figure 5: Super-pixels identifying the screw head seating as the primary influence.*

![LIME Loose](/uploads/Lime_exp2.png)
*Figure 6: LIME identifying the specific gap features that led to a 'Loose' classification.*

## Challenges & Solutions

### Challenge: Model Interpretation
**Problem**: YOLOv8’s complex architecture makes it difficult to extract traditional gradients for XAI.
**Solution**: Implemented a hook-based system to capture activations from the final bottleneck layers.

## Tech Stack Details
- **Deep Learning**: YOLOv8 (Ultralytics) for real-time detection.
- **Explainability**: Captum (LRP), LIME, and custom Grad-CAM implementations.
- **Processing**: OpenCV for image transformation and heatmap overlay.

---
**Project Status**: Completed