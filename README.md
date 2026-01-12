**Road Damage Detection & Classification (RDD2022)**

**📋 Project Overview**
This project utilizes the YOLOv8-Large (YOLOv8l) architecture to detect and classify fine-grained road anomalies. We selected the Large variant to achieve a robust balance between feature extraction capability and inference speed, specifically targeting anomalies like cracks and potholes.


**👥 Team Members**

Seshanth

Satvik Padiyala

Shafiqur Rahaman

Sathyanarayanan 

**🏗️ Model Architecture**
The model is initialized with pre-trained weights (yolov8l.pt) to leverage transfer learning.


Backbone: CSPDarknet53 for hierarchical feature learning.


Neck: PANet (Path Aggregation Network) for multi-scale feature fusion, essential for distinguishing between small alligator cracks and large longitudinal cracks.


Head: Decoupled head structure processing objectness, classification, and regression tasks independently.

Supported Classes
The network is configured to detect five specific classes:

Longitudinal Crack

Transverse Crack

Alligator Crack

Other Corruption

Pothole

**🧠 Methodology: Curriculum Learning Strategy**
We adopted a multi-stage training strategy moving from aggressive regularization to fine-grained precision tuning.

Stage 1: Robust Feature Learning (Baseline)

Goal: Prevent overfitting and learn invariant features using high-intensity augmentations.


Resolution: 640x640 pixels.


Compute: Distributed Data Parallel (DDP) across 2x NVIDIA Tesla T4 GPUs.

Augmentation:


Mosaic (0.8): High probability of stitching images to simulate complex contexts.


Mixup (0.1): Blending images to soften decision boundaries.


Scale (0.5) & Translate (0.1): Aggressive geometric distortions.

Stage 2: Precision Fine-Tuning (Refinement)

Goal: Optimize for the strict mAP50-95 metric by preserving fine structural details of cracks.



Rationale: Aggressive augmentations (like Mixup) can distort crack topology, hindering localization.


Adjustments:


Mosaic: Reduced to 0.1 to allow the model to see "whole" road sections.



Mixup: Disabled (0.0).


Scale: Reduced to 0.3 to preserve natural proportions.


Scheduler: Shifted to a Cosine learning rate scheduler for smoother convergence.
