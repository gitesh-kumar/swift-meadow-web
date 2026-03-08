---
title: "Explainable AI for End-of-Line Inspection"
date: 2026-03-07
description: "Industrial defect detection using YOLOv8 and XAI to distinguish between missing, loose, and fully tight screws."
image: "/images/xai-main.jpg"
tags: ["YOLOv8", "Computer Vision", "XAI", "Industrial AI"]
draft: false
---

### Project Overview
In automated assembly lines, missing or improperly tightened screws can lead to catastrophic failures. This project leverages **YOLOv8** for detection and multiple **XAI frameworks** to validate that the model is identifying defects based on correct mechanical features.

---

### 1. Layer-wise Relevance Propagation (LRP)
LRP provides a high-resolution pixel-wise explanation by redistributing the prediction logit back through the network.

![LRP Figure 1](content\projects\xai_eol\LRP4.png)
*Figure 1: LRP heatmap for an assembly with multiple components. Note the high relevance (red) on the screw threads for the 'Fully Tight' detection.*

![LRP Figure 2](content\projects\xai_eol\LRP6.png)
*Figure 2: LRP explanation for 'Missing Screw' class, showing how the absence of specific features triggers the model's classification.*

---

### 2. Grad-CAM (Gradient-weighted Class Activation Mapping)
Grad-CAM visualizes the "attention" of the final convolutional layers, confirming the model's focus area within the complex assembly.

![Grad-CAM Figure 1](content\projects\xai_eol\fc7a5e68-54_10_1-res.png)
*Figure 1: Heatmap showing the model's focus on the central assembly area for component verification.*

![Grad-CAM Figure 2](content\projects\xai_eol\grad_cam_loose_tight.png)
*Figure 2: Activation map during a 'Loose Screw' detection, ensuring the model isn't being distracted by background industrial textures.*

---

### 3. LIME (Local Interpretable Model-agnostic Explanations)
LIME highlights the super-pixels that most heavily influenced the YOLOv8 prediction for specific screw states.

![LIME Figure 1](content\projects\xai_eol\Lime_exp1.png)
*Figure 1: LIME explanation for a 'Fully Tight' screw, highlighting the super-pixels around the screw head seating.*

![LIME Figure 2](content\projects\xai_eol\Lime_exp2.png)
*Figure 2: LIME output for 'Loose Screw' detection, identifying the specific gap features that contributed to the model's decision.*

---

### Technical Summary
* **Detection:** YOLOv8 trained on industrial assembly images.
* **XAI Analysis:** Comparative study of LRP, Grad-CAM, and LIME to verify model reliability in safety-critical environments.
* **Deployment:** Integrated into an End-of-Line (EoL) inspection pipeline.