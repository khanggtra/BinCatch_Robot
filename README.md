# 🗑️ BinCatch: Autonomous 3D Monocular Catching Robot

## 📖 Project Overview

BinCatch is an autonomous mobile robot using a Mecanum wheel chassis to track and catch flying objects (for example, thrown trash) in mid-air. With a single monocular camera mounted on the chassis, the system predicts the 3D ballistic trajectory of the object in real-time.

The project is inspired by the research paper: **"3D Monocular Robotic Ball Catching"** (Lippiello et al., 2013).

## ✨ Key Features

- **High-Speed Monocular Vision**: Parallel visual pipeline with YOLOv8 for object detection (10–15 FPS) and OpenCV KCF/MOSSE for high-speed tracking (>100 FPS).
- **Nonlinear Trajectory Estimation**: Real-time prediction of drag and ballistic drop using Levenberg-Marquardt optimization.
- **Omnidirectional Mobility**: Four-wheel Mecanum drive controlled by an ESP32 MCU with a high-frequency PID loop.
- **Robust Chassis**: Custom 6061 aluminum sheet metal chassis for rigidity and stability during sudden acceleration.

## 🏗️ System Architecture

### Hardware Stack

- Main Computer: Raspberry Pi 5 (4 GB RAM) with active cooling.
- Microcontroller: ESP32 for motor PID loops and encoder interrupts.
- Camera: [TODO: Add camera module, e.g. Raspberry Pi Camera Module 3 / Arducam].
- Actuators: [TODO: Add motor specs, e.g. 4× 12V DC motors with quadrature encoders].
- Chassis: CNC laser-cut 6061 aluminum with PETG 3D-printed mounting brackets.

### Software Stack

- OS: Ubuntu 24.04 LTS.
- Middleware: ROS 2 Jazzy Jalisco.
- High-Level Nodes: C++ for tracking/math core, Python for YOLO inference.
- Low-Level Firmware: C++ with PlatformIO and micro-ROS.

## 📂 Repository Structure

```plaintext
BinCatch_project/
├── docs/                          # Architecture, meeting notes, references
├── hardware/                      # Mechanical CAD, electronics, 3D prints
│   ├── mechanical_cad/            # SolidWorks parts, assemblies
│   │   ├── exports/               # Fabrication files (.STEP, .DXF)
│   │   └── drawings/              # Technical drawings and GD&T
│   ├── 3d_prints/                 # Printable .STL / .3MF models
│   └── electronics/               # Schematics and PCB designs
├── firmware/                      # ESP32 PlatformIO project
│   └── esp32_mcu/
├── scripts/                       # Data collection and training scripts
└── software/                      # ROS 2 workspace
    └── ros2_ws/
        └── src/
            ├── bincatch_bringup/    # Launch files and system bring-up
            ├── bincatch_core/       # Kinematics, trajectory estimation, planning
            ├── bincatch_interfaces/ # Custom ROS 2 messages
            └── bincatch_vision/     # YOLO detector and C++ tracking node
```

## 🚀 Installation & Setup

### 1. Clone the repository

```bash
git clone https://github.com/<your_username>/BinCatch_project.git
cd BinCatch_project
```

### 2. Firmware Setup (ESP32)

1. Open `firmware/esp32_mcu` in VS Code with PlatformIO installed.
2. Connect the ESP32 board via USB.
3. Build and upload:

```bash
pio run -t upload
```

### 3. Software Setup (ROS 2 on Raspberry Pi 5)

1. Install ROS 2 Jazzy on Ubuntu 24.04.
2. Navigate to the workspace:

    ```bash
    cd software/ros2_ws/
    ```

3. Install dependencies:

    ```bash
    rosdep install --from-paths src -y --ignore-src
    ```

4. Build the workspace:

    ```bash
    colcon build --symlink-install
    ```

5. Source the environment:

    ```bash
    source install/setup.bash
    ```

## 🎮 Usage

> Update these launch commands once the system packages are finalized.

1. Start the micro-ROS agent:

    ```bash
    ros2 run micro_ros_agent micro_ros_agent serial --dev /dev/ttyUSB0
    ```

2. Launch the vision pipeline:

    ```bash
    ros2 launch bincatch_bringup vision.launch.py
    ```

3. Launch the trajectory estimator and navigation core:

    ```bash
    ros2 run bincatch_core estimator_node
    ```

## 🤝 Contributing & Git Workflow

- Hardware contributors should work only in `hardware/`.
- Firmware contributors should work only in `firmware/`.
- Software contributors should work only in `software/ros2_ws/src/`.
- Use descriptive Conventional Commit messages: `feat(vision): ...`, `fix(esp32): ...`, `chore: ...`.
- Do not push directly to `main`; create a feature branch and open a PR.

## 👥 Team Members

- Ho Chi Minh City University of Technology (HCMUT) - Mechatronics Engineering (Class CK23CDN)
- Lê Văng Sơn Hà
- Lê Thanh Hải
- Trần Gia Khang

## 📄 License

This project is licensed under the MIT License. See `LICENSE` for details.
