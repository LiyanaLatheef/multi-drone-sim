# multi-drone-sim
This project documents my journey building a multi-drone autonomous navigation and computer vision simulation using ROS 2, PX4, and Gazebo.

# Phase 1: Environment Setup (PX4 + ROS 2 + Gazebo)

## System
- OS: Ubuntu 22.04 LTS
- GPU: [fill in — you weren't sure, run `nvidia-smi` or `lspci | grep -i nvidia` to check]

## Software Versions
- ROS 2: Humble (already installed prior to this project)
- Gazebo: Classic 11.10.2 (already installed prior to this project)
- PX4-Autopilot: v1.15.0 (cloned fresh for this project)

## Installation Steps

### 1. Cloned PX4-Autopilot
​```bash
mkdir -p ~/dev && cd ~/dev
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
cd PX4-Autopilot
git checkout v1.15.0
git submodule update --init --recursive
​```

### 2. Ran PX4's dependency setup script
​```bash
bash ./Tools/setup/ubuntu.sh
​```

This installed build tools (gcc, cmake, ninja), Python dependencies, and 
additional Gazebo-related packages (including gz-garden, used by newer 
PX4 targets — not used in this project, which sticks to Gazebo Classic).

### 3. Rebooted
Required for group permission changes (`dialout` group) to take effect 
before building NuttX/simulation targets.

## Issues Encountered

### dpkg lock error during setup script
**Problem:** `E: Could not get lock /var/lib/dpkg/lock-frontend` — Ubuntu's 
background `unattended-upgrades` service was holding the lock.

**Fix:**
​```bash
sudo kill -9 <PID>
sudo dpkg --configure -a
sudo systemctl stop unattended-upgrades
sudo systemctl disable unattended-upgrades
​```
Disabled auto-updates for the duration of development to avoid future 
mid-build interruptions.

## Verification
- [ ] `make px4_sitl gazebo-classic` builds successfully
- [ ] Gazebo window opens with a quadrotor model
- [ ] `pxh>` shell prompt appears
- [ ] `commander takeoff` successfully lifts the drone in sim