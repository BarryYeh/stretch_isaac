# stretch_isaac

NVIDIA Isaac Sim 4.5.0 environment for ROS 2 testing of the SE3 Hello Robot.

## Prerequisites

- Omniverse Isaac Sim 4.5.0  
- ROS 2 (e.g. Galactic or Humble) installed and sourced  
- Python 3.10 
- NVIDIA GPU with up-to-date drivers  
- `ros2-bridge` plugin enabled in Isaac Sim  

## Directory Layout

- `Robot_Import_Files/` – modified URDF with updated collision meshes  
- `SE3_ROS2.usd/` – Issac Sim USD stage with the imported robot
- `README.md`         – this file  

## Import Process

Adapted from the Isaac Sim docs:  
- https://docs.isaacsim.omniverse.nvidia.com/4.5.0/robot_setup/import_urdf.html  
- https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/index.html  

1. **Create a new Isaac Sim project**  
2. **Import the URDF** (File > Import)  
   - Original URDF used square collision meshes on the wheels, which caused physics artifacts.  
   - Replaced them with cylinders; see `Robot_Import_Files/`.  
   - Enabled self-collision and set the base link movable.  
3. **Tune joint dynamics**  
   - **Wheels**  
     - Armature: 2.0 kg·m² (reduces jitter)  
     - Damping: 1000; Stiffness: 0  
     - Clamped max torque and brake force  
   - **Positional joints**  
     - Armature: 0.1 kg·m²  
     - Damping & stiffness hand-tuned via GUI  
       (Tools > Robotics > Asset Editors > Gain Tuner)  
4. **ROS 2 Bridge configuration** (synchronized to system time)  
   - Adapt or reuse OmniGraph templates from  
     Tools > Robotics > ROS 2 OmniGraphs  
   - Key graphs:  
     1. Camera broadcast  
     2. TF broadcast  
     3. Differential controller  
     4. Joint state publisher/subscriber  

## Launching the Simulation

```bash
# Launch Isaac Sim with Model:
./run_isaac.sh --project /path/to/SE3_Ros2.usd