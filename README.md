# yolov8-amr-perception
End-to-end YOLOv8-based object detection pipeline for Autonomous Mobile Robots (AMRs) in warehouse environments, using the LOCO dataset and industry-grade training, evaluation, and deployment practices.

# Vision-Based Perception System for Autonomous Mobile Robots (AMR)

This repository implements an **end-to-end YOLOv8-based object detection pipeline**
designed as a **vision perception module for Autonomous Mobile Robots (AMRs)**
operating in **warehouse and intralogistics environments**.

The project focuses on **real-world industrial conditions** such as clutter,
occlusions, motion blur, and varying illumination, rather than ideal benchmark
scenarios.

## Dataset

- **LOCO (Logistics Objects in Context)**
- Real warehouse imagery
- Bounding-box annotations in **COCO format**

The dataset was converted from **COCO → YOLO format** using a custom data
processing pipeline included in this repository.

## key Dependencies (LIB)
- ultralytics
- openCV2
- argparse
- pathlib
- matplotlib.pyplot
- json

## Training argument (fine tuning)
!python YOLOv8-AMR-preception/src/training/train_yolov8.py \
  --model yolov8n.pt \
  --data config/data.yaml \
  --epochs 50 \
  --imgsz 960 \
  --device 0\
  --batch 16 \
  --project models/checkpoints \
  --name amr_exp3

Best weights are automatically saved based on validation performance.
Training logs and plots are stored in "YOLOv8-AMR-preception/models/checkpoints/amr_exp3"
The 50 th epoch : 
      Epoch    GPU_mem   box_loss   cls_loss   dfl_loss  Instances       Size
      50/50      9.83G      1.686     0.9351      1.244        519        960 
                 Class     Images  Instances      Box(P          R      mAP50  mAP50-95): 
                   all       1019      31607      0.774      0.659      0.744      0.402
## Inference Argument
!python YOLOv8-AMR-preception/src/inference/infer_image.py\
  --source   YOLOv8-AMR-preception/data/yolo/images/val \
  --weights  YOLOv8-AMR-preception/models/checkpoints/amr_exp3/weights/best.pt \
  --conf 0.001\
  --iou 0.70\
  --imgsz 960\
  --device 0\
  --project  YOLOv8-AMR-preception/runs/detect \
  --name amr_val_demo

Some of the result :
image 504/1019 /content/YOLOv8-AMR-preception/data/yolo/images/val/1576595656.9178026.jpg:       608x960 1 small_load_carrier, 179 pallets, 51 stillages, 1 pallet_truck, 8.6ms
image 505/1019 /content/YOLOv8-AMR-preception/data/yolo/images/val/1576595657.2947965.jpg: 544x960 6 small_load_carriers, 288 pallets, 6 stillages, 9.1ms
image 506/1019 /content/YOLOv8-AMR-preception/data/yolo/images/val/1576595674.2849848.jpg: 736x960 4 small_load_carriers, 67 pallets, 2 stillages, 1 pallet_truck, 11.9ms


## Evaluation 
Model performance is evaluated using:
p : [    0.78331     0.80155     0.77405     0.80099     0.77123]
R : [    0.64408     0.71545     0.71399     0.69819     0.68653]
mAP50 : 0.7597630588072724
mAP50 -95 : 0.3987944127180621

Evaluation artifacts are available in:   /content/YOLOv8-AMR-preception/reports/val_latest
This project prioritizes robust and conservative detection, which is critical
for safety-aware robotic systems.

## Deployment
The trained model is exported to ONNX format for deployment.
Suitable for:
    edge devices,
    real-time RTSP camera streams,
    robotics middleware (e.g., ROS2).

## Key Takeaways
Warehouse perception is significantly harder than standard benchmarks.
Conservative detection reduces false positives and improves safety.
The pipeline is modular, reproducible, and aligned with industry practices

## Author
Udhayamoorthi Arachalakumar
M.Sc. Intelligent Manufacturing (optimization stream)
