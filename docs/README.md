# Handheld 3D Scanner Rig

A multi-sensor handheld 3D scanning rig combining LiDAR, structured IR depth, and multi-camera photogrammetry to produce high-accuracy 3D models across a wide range of object sizes. Outputs are intended for mesh editing in CAD tools (OnShape, Fusion 360) and downstream 3D printing.

---

## Project Goals

- Accurate 3D reconstruction of objects ranging from small parts (PCBs, enclosures) to large scenes (rooms, vehicles, architectural features)
- LiDAR-inertial odometry for metric scale and robust pose estimation
- Multi-camera Gaussian Splatting for high-quality appearance and geometry
- Clean mesh export suitable for CAD import and 3D printing workflows
- Modular rig design that can be upgraded incrementally

---

## Repository Structure

```
scanner-rig/
├── README.md
├── docs/
│   ├── bom.md                  # Full bill of materials with pricing
│   ├── hardware/
│   │   ├── rig-design.md       # Mechanical design, dimensions, materials
│   │   └── assembly.md         # Build instructions and adhesive notes
│   ├── software/
│   │   ├── pipeline.md         # Full data pipeline overview
│   │   └── ros2-setup.md       # ROS2 installation and configuration
│   └── calibration/
│       └── calibration.md      # Sensor calibration procedures
├── ros2/                        # ROS2 workspace (to be populated)
│   ├── src/
│   └── launch/
├── config/                      # Sensor and pipeline config files
├── scripts/                     # Utility scripts
└── cad/                         # Printable mount STLs and source files
```

---

## Sensor Suite

| Sensor | Role |
|---|---|
| Livox Horizon | Primary LiDAR — geometry and trajectory |
| FLIR BFS-U3-120S4C | Primary RGB camera — appearance, COLMAP anchor |
| Intel RealSense D435i | Active IR depth + IMU — close-range geometry, inertial odometry |
| 2× Arducam OV9281 | Flanking global shutter cameras — stereo baseline, wide coverage |
| Witmotion WT901C | Supplemental IMU (mounted near Livox) |

---

## Pipeline Overview

```
Capture
  └── LiDAR + IMU + cameras (ROS2 bag)

Step 1 — LiDAR-Inertial Odometry
  └── FAST-LIO2 → trajectory + dense point cloud

Step 2 — Camera Pose Estimation
  └── COLMAP (initialized from FAST-LIO2 trajectory)

Step 3 — Reconstruction
  └── nerfstudio / DN-Splatter → 3D Gaussian Splatting model

Step 4 — Mesh Extraction
  └── Marching cubes / Poisson → mesh

Step 5 — Mesh Cleanup
  └── Blender / Meshmixer → manifold, print-ready mesh

Step 6 — CAD Integration
  └── OnShape / Fusion 360 → model new parts around scanned geometry

Step 7 — 3D Print
  └── Slicer → printer
```

---

## Hardware Requirements

- Linux workstation with NVIDIA GPU (RTX 3080 or better, 10GB+ VRAM)
- CUDA 11.8+
- USB 3.0 hub (powered, 7-port minimum)
- 3D printer capable of ASA (enclosure recommended)

## Software Requirements

- ROS2 Humble or Iron
- FAST-LIO2
- livox_ros_driver2
- realsense-ros
- COLMAP
- nerfstudio
- Open3D
- CloudCompare (optional, GUI inspection)

See [docs/software/ros2-setup.md](docs/software/ros2-setup.md) for full installation instructions.

---

## Documentation

- [Bill of Materials](docs/bom.md)
- [Rig Design & Dimensions](docs/hardware/rig-design.md)
- [Assembly Guide](docs/hardware/assembly.md)
- [Software Pipeline](docs/software/pipeline.md)
- [ROS2 Setup](docs/software/ros2-setup.md)
- [Calibration Procedures](docs/calibration/calibration.md)