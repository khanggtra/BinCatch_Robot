# 🗑️ BinCatch: Autonomous 3D Monocular Catching Robot

![ROS 2](https://img.shields.io/badge/ROS_2-Jazzy-blue)
![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04-orange)
![Hardware](https://img.shields.io/badge/Hardware-Raspberry_Pi_5_%7C_STM32-green)
![Status](https://img.shields.io/badge/Status-Under_Development-yellow)

## 📖 Project Overview

BinCatch is an autonomous mobile robot using a Mecanum wheel chassis to track and catch flying objects (such as thrown trash) in mid-air. With a single monocular camera mounted on the chassis, the system predicts the 3D ballistic trajectory of the object in real-time.

The core concept and mechanics of this project take direct inspiration from the viral YouTube video: **["We Built an Auto-aiming Trash Can" by HTX Studio](https://www.youtube.com/watch?v=H0XYANRosVo)**. We aim to recreate and enhance this mid-air trash-catching mechanism by applying rigorous mathematical trajectory estimations and industrial-grade control architecture.

## ✨ Key Features

* **High-Speed Monocular Vision:** Parallel visual pipeline with YOLOv8 for object detection (10–15 FPS) and OpenCV KCF/MOSSE for high-speed tracking (>100 FPS).
* **Nonlinear Trajectory Estimation:** Real-time prediction of aerodynamic drag and ballistic drop using Levenberg-Marquardt optimization.
* **Omnidirectional Mobility:** Four-wheel Mecanum drive controlled by a custom **STM32 MCU** board, leveraging hardware timer encoder interfaces for high-frequency PID loops.
* **Robust Chassis:** Custom 6061 aluminum sheet metal chassis for rigidity and stability during sudden acceleration.

## 🏗️ System Architecture

### Hardware Stack

* **Main Computer:** Raspberry Pi 5 (4 GB RAM) with active cooling.
* **Microcontroller:** Custom-designed STM32 (e.g., STM32F4 series) Minimum System Board for motor PID loops, hardware encoder decoding, and DMA/UART communication.
* **Camera:** [TODO: Add camera module, e.g. Raspberry Pi Camera Module 3 / Arducam].
* **Actuators:** [TODO: Add motor specs, e.g. 4× 12V DC motors with quadrature encoders].
* **Chassis:** CNC laser-cut 6061 aluminum with PETG 3D-printed mounting brackets.

### Software Stack

* **OS:** Ubuntu 24.04 LTS.
* **Middleware:** ROS 2 Jazzy Jalisco.
* **High-Level Nodes:** C++ for tracking/math core, Python for YOLO inference.
* **Low-Level Firmware:** C/C++ with STM32CubeMX, HAL Library, and micro-ROS.

## 📂 Repository Structure

```text
BinCatch_project/
├── docs/                          # Architecture, meeting notes, references
├── hardware/                      # Mechanical CAD, electronics, 3D prints
│   ├── mechanical_cad/            # SolidWorks parts, assemblies
│   │   ├── exports/               # Fabrication files (.STEP, .DXF)
│   │   └── drawings/              # Technical drawings and GD&T
│   ├── pcb_design/                # Custom STM32 Altium/KiCad schematic layouts
│   └── 3d_prints/                 # Printable .STL / .3MF models
├── firmware/                      # Microcontroller Firmware
│   └── stm32_mcu/                 # STM32CubeMX generated project (Core, HAL, micro-ROS)
├── scripts/                       # Data collection and YOLO training scripts
└── software/                      # ROS 2 workspace
    └── ros2_ws/
        └── src/
            ├── bincatch_bringup/    # Launch files and system bring-up
            ├── bincatch_core/       # Kinematics, trajectory estimation, planning
            ├── bincatch_interfaces/ # Custom ROS 2 messages (.msg)
            └── bincatch_vision/     # YOLO detector and C++ tracking node
```

## 🚀 Installation & Setup

### 1. Clone the repository

```bash
git clone [https://github.com/](https://github.com/)<your_username>/BinCatch_project.git
cd BinCatch_project
```

### 2. Firmware Setup (STM32)

**Step 1:** Open `firmware/stm32_mcu` project using **STM32CubeIDE** or VS Code.

**Step 2:** Connect the custom STM32 board via an ST-Link V2 programmer.

**Step 3:** Build the project and flash the firmware onto the MCU.

### 3. Software Setup (ROS 2 on Raspberry Pi 5)

**Step 1:** Install ROS 2 Jazzy on Ubuntu 24.04.

**Step 2:** Navigate to the workspace:

```bash
cd software/ros2_ws/
```

**Step 3:** Install dependencies:

```bash
rosdep install --from-paths src -y --ignore-src
```

**Step 4:** Build the workspace:

```bash
colcon build --symlink-install
```

**Step 5:** Source the environment:

```bash
source install/setup.bash
```

## 🎮 Usage

> Update these launch commands once the system packages are finalized.

**1. Start the micro-ROS agent (via UART/DMA):**

```bash
ros2 run micro_ros_agent micro_ros_agent serial --dev /dev/ttyUSB0
```

**2. Launch the vision pipeline:**

```bash
ros2 launch bincatch_bringup vision.launch.py
```

**3. Launch the trajectory estimator and navigation core:**

```bash
ros2 run bincatch_core estimator_node
```

## 🤝 Contributing & Git Workflow

* Hardware contributors should work only in `hardware/`.
* Firmware contributors should work only in `firmware/`.
* Software contributors should work only in `software/ros2_ws/src/`.
* Use descriptive Conventional Commit messages: `feat(vision): ...`, `fix(stm32): ...`, `chore: ...`.
* Do not push directly to `main`; create a feature branch and open a PR.

## 👥 Team Members

* **Lê Văng Sơn Hà:** [TODO: Update Role - e.g., Mechanical Design & PCB Design]
* **Lê Thanh Hải:** [TODO: Update Role - e.g., Computer Vision & AI Training]
* **Trần Gia Khang :** High-Level Control Architecture (ROS 2), Trajectory Estimation Math

## 📄 License

This project is licensed under the MIT License. See `LICENSE` for details.
