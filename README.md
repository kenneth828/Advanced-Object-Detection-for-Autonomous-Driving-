# Advanced Object Detection for Autonomous Driving

Deep learning-based object detection system comparing YOLOv5 and Faster R-CNN for real-time road event awareness in autonomous vehicles.

## 📋 Project Overview

This project implements and evaluates two state-of-the-art object detection models on the ROAD (Road Event Awareness Dataset) to address the critical challenge of situation awareness in autonomous driving. The system detects and classifies 11 object classes including vehicles, pedestrians, cyclists, and traffic lights in real-world driving scenarios.

## 🎯 Objectives

- Implement and train YOLOv5 (single-stage detector) for real-time object detection
- Implement and train Faster R-CNN (two-stage detector) for high-accuracy detection
- Compare performance across different object sizes and classes
- Analyze model behavior on real-world autonomous driving scenarios
- Evaluate trade-offs between speed and accuracy for autonomous vehicle applications

## 🏗️ Models Implemented

### YOLOv5s
- **Architecture**: Single-stage detector with CSPDarknet53 backbone
- **Parameters**: 7.2 million
- **Advantages**: Fast inference, suitable for real-time applications
- **mAP@0.5**: 54.1%
- **Best Classes**: Pedestrian (84.9%), Cyclist (81.3%), Vehicle-traffic-light (80.8%)

### Faster R-CNN with ResNet-50
- **Architecture**: Two-stage detector with Region Proposal Network (RPN)
- **Backbone**: ResNet-50 pre-trained on COCO
- **Advantages**: Higher localization accuracy, especially for large objects
- **mAP@0.5**: 56%
- **Large Object AP**: 61%

## 📊 Dataset: ROAD (Road Event Awareness Dataset)

- **Total Frames**: 122,000 annotated frames across 22 long videos
- **Labels**: 1.7 million labels, 560,000 bounding boxes
- **Classes**: 11 object classes
  - Autonomous-vehicle, Car, Cyclist, Motorbike
  - Medium-vehicle, Large-vehicle, Bus
  - Pedestrian, Vehicle-traffic-light
  - Emergency-vehicle, Other-traffic-light
- **Context**: Real-world driving conditions with varying weather and lighting
- **Format**: YOLO format for YOLOv5, COCO format for Faster R-CNN

## 🚀 Results

### YOLOv5s Performance (Test Set)

| Metric | Value |
|--------|-------|
| Overall mAP@0.5 | 54.1% |
| Overall mAP@0.5:0.95 | 24.9% |
| Precision | 64% |
| Recall | 53.4% |

**Per-Class Performance:**
- Pedestrian: 84.9% mAP@0.5
- Cyclist: 81.3% mAP@0.5
- Vehicle-traffic-light: 80.8% mAP@0.5
- Car: 74.6% mAP@0.5

### Faster R-CNN Performance (Test Set)

| Metric | Value |
|--------|-------|
| Overall mAP@0.5 | 56% |
| Overall mAP@0.5:0.95 | 31% |
| Small Objects AP | 26% |
| Medium Objects AP | 32% |
| Large Objects AP | 61% |

## 🔬 Key Findings

1. **Speed vs Accuracy Trade-off**: Faster R-CNN achieved 1.9% higher mAP@0.5 than YOLOv5 but with significantly longer inference time, making YOLOv5 more suitable for real-time autonomous driving applications.

2. **Object Size Impact**: 
   - Faster R-CNN excels at large objects (61% AP) but struggles with small objects (26% AP)
   - YOLOv5 provides more balanced performance across different object sizes

3. **Class Imbalance Challenge**: Both models struggled with low-instance classes (e.g., Motorbike: 86 instances, Bus: 588 instances) compared to high-instance classes (e.g., Autonomous-vehicle: 14,043 instances)

4. **Training Stability**: YOLOv5 showed potential overfitting with validation mAP@0.5 peaking at epoch 5 (46.1%) then fluctuating, while training loss continued to decrease



## 📊 Training Configuration

### YOLOv5s Hyperparameters
- **Image Size**: 640×640
- **Batch Size**: 32
- **Epochs**: 20
- **Learning Rate**: 0.01 (initial)
- **Optimizer**: SGD with momentum
- **Pre-trained Weights**: yolov5s.pt

### Faster R-CNN Hyperparameters
- **Batch Size**: 4
- **Learning Rate**: 0.005
- **Epochs**: 20
- **Optimizer**: SGD (momentum=0.9, weight_decay=0.0005)
- **LR Scheduler**: StepLR (step_size=3, gamma=0.1)
- **Backbone**: ResNet-50 (pre-trained on COCO)

## 🔍 Challenges & Solutions

### Challenges Faced:
1. **Class Imbalance**: Dataset heavily skewed toward certain classes
2. **Computational Limitations**: Limited GPU memory 
3. **Small Object Detection**: Difficulty detecting small objects at 640×640 resolution
4. **Training Time**: Faster R-CNN required 24+ hours for full training

### Solutions Implemented:
1. **Class Weighting**: Applied computed class weights to balance training
2. **Early Stopping**: Implemented early stopping (patience=2) to prevent overfitting
3. **MPS Acceleration**: Utilized Metal Performance Shaders for M1 Mac
4. **Batch Size Optimization**: Adjusted batch sizes to fit memory constraints



## 📖 References

1. Singh, G., et al. (2023). "ROAD: The Road Event Awareness Dataset for Autonomous Driving." IEEE Transactions on Pattern Analysis and Machine Intelligence.

2. Ren, S., et al. (2015). "Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks." NeurIPS.

3. Ultralytics YOLOv5. Available at: https://github.com/ultralytics/yolov5

4. Mostafa, T., et al. (2022). "Occluded Object Detection for Autonomous Vehicles Employing YOLOv5, YOLOX and Faster R-CNN."

