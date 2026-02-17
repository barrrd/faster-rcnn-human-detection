# 🧍‍♂️ Faster R-CNN Human Detection (Trained from Scratch)

A computer vision project focused on localizing humans in images using bounding boxes. This project tackles the challenge of training a robust object detector **entirely from scratch** without relying on any pretrained weights.

### 📂 Access Key Materials
* 📄 **Project Report (PDF)**: [View Report in docs/report.pdf](./docs/report.pdf)

---

## 🌟 Project Objective & Challenges

The main objective is to accurately detect humans in various poses, scales, and occlusions amidst cluttered backgrounds. 
**Key Challenge Constraint:** The use of pretrained models was strictly prohibited. The model had to be trained from scratch using limited resources, making convergence and robust feature extraction highly challenging.

---

## 🧠 Methodology & Architecture

* **Architecture**: Faster R-CNN with a MobileNetV3-Large FPN backbone.
* **Implementation Details**:
  * **From Scratch Initialization**: Instantiated `fasterrcnn_mobilenet_v3_large_fpn` from TorchVision with `weights=None` and `weights_backbone=None`. All trainable parameters (Backbone, FPN, RPN, RoI Heads) were randomly initialized.
  * **Classification Head**: Modified the `FastRCNNPredictor` to support exactly 2 classes (Human and Background).
* **Training Configuration**:
  * **Losses**: Standard Faster R-CNN multi-task loss (Classification, Bbox Regression, RPN Objectness, RPN Box Regression).
  * **Optimizer**: Adam (lr=0.0005, betas=(0.9, 0.999), weight_decay=0.0005).

---

## 📈 Performance (Results)

* **Evaluation Metric**: 11-point interpolated mAP@0.5 (Mean Average Precision with IoU threshold 0.5).
* **mAP@0.5 Score**: **`0.5431`**
* **Inference Time**: ~119.50 seconds

Considering the model was trained entirely from scratch without prior semantic knowledge, achieving an mAP of 0.54 demonstrates the effective learning of human features.

---

## 🔍 Insights & Limitations

Through rigorous evaluation, several trade-offs and areas for improvement were identified:
1. **Inference Speed**: The total evaluation time (~119.5s) exceeded the strict 90-second challenge constraint. This is inherently due to the two-stage nature of Faster R-CNN (RPN + RoI Heads), which is naturally slower than single-stage detectors.
2. **Bounding Box Misalignment**: Some predicted boxes were noticeably larger or smaller than the ground-truth annotations, indicating that the regressor occasionally struggled to converge tightly on ambiguous edges.
3. **False Positives (Generalization Issue)**: Without pretrained semantic priors, the model occasionally responded to human-like shapes or postures. For instance, the model mistakenly classified a giraffe as a human in one of the test cases.

**Future Work**: Adopting faster single-stage architectures (e.g., YOLO, SSD) and incorporating hard negative mining could significantly reduce non-human confusion and improve processing speed.

---

## 📂 Repository Structure

```text
faster-rcnn-human-detection/
├── docs/                  
│   └── report.pdf         # Detailed project report
├── HumanDetection.ipynb   # Main notebook (Data processing, Training, Evaluation)
├── HumanDetection.pt      # Trained model weights (Trained from scratch)
└── README.md              # Project overview