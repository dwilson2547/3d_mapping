# ROS2 Setup

## System Requirements

- Ubuntu 22.04 LTS (recommended — best ROS2 Humble compatibility)
- NVIDIA GPU with CUDA 11.8+ (RTX 3080 or better)
- 32GB RAM recommended for large scene processing
- USB 3.0 ports (or hub) for camera connections
- Gigabit Ethernet for Livox Horizon

---

## ROS2 Installation (Humble)

```bash
# Set locale
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

# Add ROS2 apt repository
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
  -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
  http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" \
  | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Install ROS2 Humble Desktop
sudo apt update
sudo apt install ros-humble-desktop

# Source ROS2 (add to ~/.bashrc)
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

---

## Workspace Setup

```bash
mkdir -p ~/scanner_ws/src
cd ~/scanner_ws
colcon build
source install/setup.bash
echo "source ~/scanner_ws/install/setup.bash" >> ~/.bashrc
```

---

## Driver Installation

### Livox Horizon — livox_ros_driver2

```bash
cd ~/scanner_ws/src
git clone https://github.com/Livox-SDK/livox_ros_driver2.git
cd ..
colcon build --packages-select livox_ros_driver2
source install/setup.bash
```

Configure the Livox IP address in `config/MID360_config.json` (Horizon uses a similar config):
- Default Livox IP: `192.168.1.1XX` (check sticker on unit)
- Set host IP to `192.168.1.50` (or configure your ethernet interface accordingly)

```bash
# Set ethernet interface static IP
sudo ip addr add 192.168.1.50/24 dev eth0
```

### Intel RealSense D435i — realsense-ros

```bash
# Install librealsense2
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCD
sudo add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main"
sudo apt update
sudo apt install librealsense2-dkms librealsense2-utils librealsense2-dev

# Install realsense-ros
sudo apt install ros-humble-realsense2-camera ros-humble-realsense2-description
```

Test connection:
```bash
realsense-viewer  # verify D435i is detected
ros2 launch realsense2_camera rs_launch.py  # verify ROS2 topics publish
```

### FLIR Blackfly S — Spinnaker SDK + ros2_flir

```bash
# Download Spinnaker SDK from FLIR website (requires account)
# https://www.flir.com/products/spinnaker-sdk/
# Install .deb package:
sudo dpkg -i spinnaker-*.deb
sudo apt install -f

# Add user to flirimaging group
sudo adduser $USER flirimaging

# Install flir_camera_driver
sudo apt install ros-humble-flir-camera-driver
```

Verify camera detected:
```bash
SpinView  # FLIR GUI viewer, should show camera
```

### Arducam OV9281 — UVC (plug and play)

OV9281 USB variants use standard UVC drivers — no additional driver installation needed on Ubuntu 22.04.

```bash
# Verify cameras appear as video devices
ls /dev/video*

# Test with v4l2
sudo apt install v4l-utils
v4l2-ctl --list-devices

# Install usb_cam driver for ROS2
sudo apt install ros-humble-usb-cam
```

### Witmotion WT901C IMU

```bash
# Install witmotion ROS2 driver
cd ~/scanner_ws/src
git clone https://github.com/ElettraSciComp/witmotion_IMU_ros.git
cd ..
colcon build --packages-select witmotion_ros
```

Configure serial port in launch file (default `/dev/ttyUSB0`):
```bash
# Add user to dialout group for serial access
sudo adduser $USER dialout
# Re-login or run: newgrp dialout
```

---

## FAST-LIO2 Installation

```bash
# Dependencies
sudo apt install ros-humble-pcl-ros ros-humble-tf2-eigen

# Clone and build
cd ~/scanner_ws/src
git clone --recursive https://github.com/hku-mars/FAST_LIO.git
cd ..
colcon build --packages-select fast_lio
```

Copy and edit config:
```bash
cp ~/scanner_ws/src/FAST_LIO/config/livox_lio_PA.yaml \
   ~/scanner_ws/src/FAST_LIO/config/horizon_lio.yaml
```

Key parameters to set in `horizon_lio.yaml`:
```yaml
lidar_type: 6                    # Livox Horizon
scan_line: 6
blind: 0.5
point_filter_num: 1
time_offset_lidar_to_imu: 0.0   # Calibrate this value
extrinsic_T: [0.0, 0.0, 0.1]   # LiDAR to IMU translation (meters) — measure from rig
extrinsic_R: [1,0,0, 0,1,0, 0,0,1]  # LiDAR to IMU rotation — calibrate
```

---

## COLMAP Installation

```bash
sudo apt install colmap
# or build from source for latest features:
# https://colmap.github.io/install.html
```

Verify CUDA support:
```bash
colmap --version  # should show CUDA availability
```

---

## nerfstudio Installation

```bash
# Requires CUDA 11.8+ and PyTorch
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118

# Install nerfstudio
pip install nerfstudio

# Verify installation
ns-train --help
```

---

## Launch Files

### Full Rig Launch

```bash
# Launch all sensors simultaneously
ros2 launch scanner_rig full_rig.launch.py
```

A `full_rig.launch.py` file should be created in `ros2/launch/` that starts:
- `livox_ros_driver2`
- `realsense2_camera` (with IMU enabled)
- `flir_camera_driver`
- `usb_cam` (×2 for OV9281s)
- `witmotion_ros`
- Static TF publishers for all sensor extrinsics

### Record Bag

```bash
ros2 bag record \
  /livox/lidar \
  /livox/imu \
  /camera/imu \
  /camera/depth/points \
  /camera/color/image_raw \
  /flir/image_raw \
  /ov9281/left/image_raw \
  /ov9281/right/image_raw \
  /imu/witmotion \
  /tf \
  /tf_static \
  -o scan_$(date +%Y%m%d_%H%M%S)
```

---

## Useful ROS2 Commands

```bash
# List active topics
ros2 topic list

# Check topic frequency (verify all sensors publishing)
ros2 topic hz /livox/lidar
ros2 topic hz /flir/image_raw

# Visualize in RViz2
rviz2

# Play back recorded bag
ros2 bag play scan_20240101_120000/

# Inspect bag contents
ros2 bag info scan_20240101_120000/
```