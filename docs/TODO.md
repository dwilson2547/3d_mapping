# Project Checklist

Track progress through each phase of the scanner rig build. Work roughly top to bottom — later phases depend on earlier ones.

---

## Phase 1 — Procurement

### Sensors
- [ ] FLIR Blackfly S BFS-U3-120S4C
- [ ] Intel RealSense D435i
- [ ] Arducam OV9281 USB ×2 (confirm hardware trigger pin variant before ordering)
- [ ] Witmotion WT901C IMU

### Optics
- [ ] M12 6mm lens ×2 (matched pair, same batch) for OV9281s
- [ ] C-mount lens for FLIR (6–12mm prime — Computar or Kowa)

### Electronics
- [ ] Powered USB 3.0 hub (7-port industrial)
- [ ] ESP32 dev board (camera sync controller)
- [ ] USB-A to USB-C cables 0.5m ×4
- [ ] USB-C to USB-A cable 1m ×1
- [ ] Ethernet cable 1m ×1 (Livox to workstation)
- [ ] Velcro cable ties

### Mechanical
- [ ] Structural epoxy (Loctite EA 9492 or 3M DP420)
- [ ] Self-amalgamating tape
- [ ] M3 hex socket cap screws assortment
- [ ] M4 hex socket cap screws assortment
- [ ] M3 heat-set nut-serts
- [ ] M4 heat-set nut-serts

### Consumables & Safety
- [ ] ASA filament (1kg minimum)
- [ ] PLA filament (test prints)
- [ ] 220 grit sandpaper
- [ ] Isopropyl alcohol 99%
- [ ] N95 respirator (required for CF cutting)
- [ ] Nitrile gloves

### Calibration Targets
- [ ] ChArUco board printed A3, laminated (8×6 grid, 30mm squares, 20mm markers)
- [ ] AprilTag grid target printed at known dimensions (verify with calipers)

---

## Phase 2 — Mechanical Design & Test Prints

### Center Hub
- [ ] Design center hub in CAD with three bores (2× square arm, 1× round handle)
- [ ] Bore engagement depth ≥30mm on all three ports
- [ ] Gusset/fillet between vertical and horizontal bores
- [ ] Cable routing clearance
- [ ] Slip fit tolerance (0.1–0.2mm clearance on all bores)
- [ ] Livox bracket mounting holes (M4) on top face
- [ ] Test print in PLA — verify CF tube fits all bores
- [ ] Final print in ASA

### Livox Bracket
- [ ] Design bracket to sleeve over center hub top
- [ ] Matches Livox Horizon mounting hole pattern (M4)
- [ ] 5–10° forward/downward tilt baked into mount angle
- [ ] Test print in PLA
- [ ] Final print in ASA

### OV9281 Split Clamp Mounts ×2
- [ ] Design two-piece split clamp for 12mm square CF tube
- [ ] Clamping bolt perpendicular to tube axis (M3)
- [ ] M3 nut-sert pockets in clamp halves
- [ ] Camera mounting face with M3 nut-serts
- [ ] 5–10° toe-in angle baked into camera mounting face
- [ ] Test print in PLA — verify clamp action and camera fit
- [ ] Final print in ASA ×2

### FLIR Mount
- [ ] Design fixed mount for FLIR (epoxy bonded to hub or arm)
- [ ] M4 or 1/4"-20 camera mounting hole pattern
- [ ] Test print in PLA
- [ ] Final print in ASA

### D435i Mount
- [ ] Design fixed mount using D435i 1/4"-20 tripod socket
- [ ] Position as close to directly below Livox as possible
- [ ] Test print in PLA
- [ ] Final print in ASA

### Cable Management
- [ ] Design cable clips for along both arms (snap or friction fit)
- [ ] Test print in PLA
- [ ] Final print in ASA

### Handle
- [ ] Determine comfortable grip length (~100–120mm)
- [ ] Design any grip texture or contour into round tube sleeve (optional)
- [ ] Check center of mass with all sensors mounted — adjust if rig is too top-heavy

---

## Phase 3 — Mechanical Assembly

### Carbon Fiber Preparation
- [ ] Measure and mark cut lines on both round and square CF tubes
- [ ] Set up wet cutting (water or damp cloth to suppress dust)
- [ ] Put on N95 respirator before cutting
- [ ] Cut round tube to handle length
- [ ] Cut square tubes to arm lengths (×2)
- [ ] Lightly sand all bonding surfaces with 220 grit
- [ ] Wipe all bonding surfaces with IPA — allow to fully dry

### Assembly — Fixed Joints (Epoxy)
- [ ] Dry fit all tubes into center hub — mark insertion depth with tape
- [ ] Mix epoxy per instructions
- [ ] Bond square arm tubes into center hub (both sides simultaneously)
- [ ] Bond round handle tube into center hub bottom
- [ ] Align, clamp, leave undisturbed 24hrs
- [ ] Verify full cure before applying load (72hrs recommended)

### Sensor Mounting
- [ ] Mount Livox bracket to center hub top (M4 bolts)
- [ ] Mount Livox Horizon to bracket (M4 bolts, torque evenly)
- [ ] Epoxy FLIR mount in position, allow to cure
- [ ] Mount FLIR camera to mount (M4 or 1/4"-20)
- [ ] Epoxy D435i mount in position, allow to cure
- [ ] Mount D435i to mount (1/4"-20)
- [ ] Slide OV9281 split clamps onto square arm tubes
- [ ] Mount OV9281 cameras to clamps
- [ ] Set toe-in angle on each OV9281 (5–10° toward center)
- [ ] Temporarily tighten clamps — do not final-torque until after calibration

### Wiring
- [ ] Mount ESP32 sync controller to rig or hub enclosure
- [ ] Run GPIO sync lines from ESP32 to FLIR hardware trigger pin
- [ ] Run GPIO sync lines from ESP32 to OV9281 L hardware trigger pin
- [ ] Run GPIO sync lines from ESP32 to OV9281 R hardware trigger pin
- [ ] Connect all USB cameras to powered hub
- [ ] Connect D435i to hub (USB 3.0)
- [ ] Connect FLIR to hub (USB 3.0)
- [ ] Connect OV9281 ×2 to hub (USB)
- [ ] Connect Witmotion IMU to hub (USB)
- [ ] Connect ESP32 to hub (USB)
- [ ] Connect Livox Horizon via Ethernet to workstation
- [ ] Route and secure all cables with Velcro ties and printed clips
- [ ] Verify no cables hang into any camera's field of view

---

## Phase 4 — Software Setup

### Workstation
- [ ] Ubuntu 22.04 LTS installed
- [ ] NVIDIA drivers installed and verified (`nvidia-smi`)
- [ ] CUDA 11.8+ installed and verified (`nvcc --version`)
- [ ] Git configured

### ROS2
- [ ] ROS2 Humble installed
- [ ] `source /opt/ros/humble/setup.bash` added to `~/.bashrc`
- [ ] ROS2 workspace created (`~/scanner_ws/src`)

### Sensor Drivers
- [ ] `livox_ros_driver2` built and verified (Livox topics publishing)
- [ ] `realsense-ros` installed and verified (D435i topics publishing)
- [ ] Spinnaker SDK installed and verified (FLIR visible in SpinView)
- [ ] `flir_camera_driver` installed and verified
- [ ] `usb_cam` installed, both OV9281s publishing
- [ ] `witmotion_ros` built and verified (IMU topic publishing)
- [ ] User added to `flirimaging` and `dialout` groups

### FAST-LIO2
- [ ] `fast_lio` built in workspace
- [ ] `horizon_lio.yaml` config created
- [ ] Test run against a short static bag — verify point cloud output

### COLMAP
- [ ] COLMAP installed
- [ ] CUDA support verified in COLMAP

### nerfstudio
- [ ] PyTorch installed with CUDA support
- [ ] nerfstudio installed (`ns-train --help` works)
- [ ] Test run on sample dataset

### Open3D
- [ ] Open3D installed (`pip install open3d`)
- [ ] Verify point cloud read/write works

### GitHub Repository
- [ ] Repository created
- [ ] Documentation files committed (README, BOM, rig-design, pipeline, ros2-setup, calibration)
- [ ] `.gitignore` configured (exclude rosbags, large point clouds, model outputs)
- [ ] Checklist committed
- [ ] `ros2/`, `config/`, `scripts/`, `cad/` directories created with `.gitkeep`

### Launch Files
- [ ] `full_rig.launch.py` written (starts all sensor drivers)
- [ ] Static TF publishers for all sensors added to launch file
- [ ] Bag recording script written or documented
- [ ] Launch file committed to repo

---

## Phase 5 — Calibration

### Camera Intrinsics
- [ ] FLIR intrinsics calibrated (50+ frames, full image coverage)
- [ ] OV9281 left intrinsics calibrated
- [ ] OV9281 right intrinsics calibrated
- [ ] Calibration YAML files saved to `config/`

### Camera-to-Camera Extrinsics (Kalibr)
- [ ] Kalibr installed (Docker or source)
- [ ] Multi-camera calibration bag recorded
- [ ] Kalibr calibration run — `camchain.yaml` generated
- [ ] `camchain.yaml` saved to `config/`

### LiDAR-to-Camera Extrinsic
- [ ] `livox_camera_calib` built
- [ ] Physical offset between Livox and FLIR measured (initial guess)
- [ ] Calibration bag recorded (static AprilTag scene)
- [ ] Calibration run — LiDAR-to-FLIR transform generated
- [ ] Result saved to `config/lidar_to_flir.yaml`

### LiDAR-to-IMU Extrinsic
- [ ] `lidar_imu_calib` built
- [ ] Calibration bag recorded (varied motion — figure-8, tilts)
- [ ] Calibration run — transform generated
- [ ] Result saved to `config/lidar_to_imu.yaml`
- [ ] Values copied into `fast_lio/config/horizon_lio.yaml`

### IMU Noise Model
- [ ] `allan_variance_ros` built
- [ ] 2hr+ static IMU bag recorded
- [ ] Allan variance computed — noise parameters generated
- [ ] Parameters saved to `config/imu_param.yaml`
- [ ] Values copied into FAST-LIO2 config

### Scale Verification
- [ ] Known-dimension object scanned (ruler or calibration bar)
- [ ] Distance measured in CloudCompare
- [ ] Scale error confirmed under 0.5%

### Final Steps
- [ ] OV9281 split clamps final-torqued to lock camera positions
- [ ] All calibration files committed to repo with calibration date in commit message

---

## Phase 6 — Pipeline Validation

### First Full Capture
- [ ] Full rig launch verified (all topics publishing)
- [ ] Short indoor test bag recorded (~2–3 minutes, varied motion)
- [ ] Bag plays back cleanly with all topics present

### Odometry
- [ ] FAST-LIO2 run on test bag
- [ ] Trajectory and point cloud output verified in RViz2
- [ ] No obvious drift or discontinuities in trajectory

### Camera Poses
- [ ] Images extracted from test bag (FLIR + OV9281s)
- [ ] COLMAP feature extraction and matching completed
- [ ] COLMAP mapper run — sparse reconstruction produced
- [ ] Camera poses visually verified in COLMAP GUI

### LiDAR + Camera Fusion
- [ ] FAST-LIO2 cloud registered to COLMAP sparse cloud (ICP)
- [ ] Alignment visually verified in CloudCompare or Open3D

### Reconstruction
- [ ] nerfstudio data prepared from COLMAP output
- [ ] 3DGS training run on test dataset
- [ ] Model renders verified (no obvious artifacts, correct geometry)

### Mesh Export
- [ ] Mesh exported from nerfstudio
- [ ] Basic mesh cleanup run in Open3D (remove degenerate triangles, smooth)
- [ ] Mesh inspected in Blender — manifold check with 3D Print Toolbox
- [ ] Mesh exported as `.stl`

---

## Phase 7 — Scan-to-Print Workflow Validation

### CAD Integration
- [ ] Test mesh imported into OnShape as reference geometry
- [ ] Simple parametric part modeled around mesh (e.g., a bracket or mount)
- [ ] Boolean operation with mesh geometry tested (fitted cavity)
- [ ] Part exported as `.stl`

### 3D Print
- [ ] Exported part sliced
- [ ] Test print completed
- [ ] Physical fit against scanned object verified

### Documentation
- [ ] Any pipeline issues discovered documented in repo
- [ ] Utility scripts written for steps that are manual/repetitive
- [ ] Scripts committed to `scripts/`
- [ ] `README.md` updated with any changes from the build process

---

## Future Upgrades (Backlog)

- [ ] Additional FLIR cameras to replace OV9281s (upgrade path ×2)
- [ ] Structured light IR projector for textureless surfaces
- [ ] Polarizing filter on FLIR
- [ ] High CRI LED ring illuminator (5600K, dimmable)
- [ ] VL53L5CX ToF array for close-range proximity
- [ ] RTK GPS (u-blox F9P) for outdoor metric scale
- [ ] Automated turntable rig for small object scanning (ESP32 + stepper)
- [ ] Zivid Two M130 for close-range structured light (handheld rig)
- [ ] Photoneo PhoXi 3D Scanner S for dedicated small object / PCB station
- [ ] LVI-SAM for tighter visual-inertial-LiDAR coupling (pipeline upgrade)
- [ ] DN-Splatter integration for better textureless surface reconstruction