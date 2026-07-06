
## Project title - **Parametric Robotic Welding Digital Twin with ROS 2, Gazebo, Motor Sizing, and AI Agent Control**

The idea was to build a complete workflow where you design a robot from scratch, simulate it, size the mechanical/electrical components, then allow an AI agent to control or modify parts of the system.

## Stage 1: Parametric CAD robot design

Create the robot in **FreeCAD**, **Creo**, or another CAD tool in a way that is fully parametric.

The key requirement was that the robot geometry should be driven by an editable file, such as:

```text
robot_config.yaml
robot_dimensions.json
parameters.csv
```

Example parameters:

```text
base_length
base_width
link_1_length
link_2_length
joint_spacing
mounting_plate_thickness
motor_mount_size
end_effector_offset
```

The goal was that an AI agent could eventually change the robot design by editing the parameter file, rather than manually changing the CAD model.

Deliverables:

```text
/01_parametric_cad
  robot_config.yaml
  robot_model.FCStd or .step
  generated_parts/
  design_notes.md
```

## Stage 2: Motor and actuator sizing

Use the CAD geometry to calculate the worst-case forces and torques.

This stage would work out:

```text
joint torque requirements
motor torque requirements
gearbox ratios
safety factors
payload limits
worst-case arm positions
static and dynamic loading
```

The aim was to build a calculation tool where you could change robot dimensions or payload and automatically estimate whether the motors are large enough.

Deliverables:

```text
/02_motor_sizing
  torque_calculator.py
  motor_sizing.xlsx
  worst_case_positions.md
  selected_motors.md
```

Basic logic:

```text
CAD dimensions → link lengths and masses
Payload → worst-case joint loading
Torque equations → required motor torque
Safety factor → selected motor/gearbox
```

## Stage 3: URDF, ROS 2, and Gazebo simulation

Convert the robot into a **URDF/Xacro** model and simulate it in **ROS 2 + Gazebo**.

This stage would include:

```text
robot links
joints
joint limits
mass/inertia values
collision geometry
visual geometry
ros2_control
Gazebo simulation
basic movement testing
```

The point was to create a digital twin of the robot that could be tested before building hardware.

Deliverables:

```text
/03_ros2_gazebo
  robot_description/
    urdf/
    meshes/
    launch/
  robot_control/
  robot_bringup/
  gazebo_worlds/
```

Main outcome:

```text
You can spawn the robot in Gazebo and move its joints from ROS 2.
```

## Stage 4: AI agent robot control

Build an AI-agent layer that can command the robot to move to specified positions.

The AI agent would not directly “guess” motor commands. It would interact with safer tools/scripts, for example:

```text
move_to_position(x, y, z)
set_joint_angles(j1, j2, j3...)
run_simulation()
check_collision()
generate_motion_plan()
```

The idea was:

```text
User instruction → AI agent → ROS 2 control script → robot moves in simulation
```

Example instruction:

```text
Move the end effector 300 mm forward and 100 mm up.
```

The agent would convert that into a planned movement, test it, and send it through ROS 2.

Deliverables:

```text
/04_ai_agent_control
  agent_tools/
  motion_commands.py
  safe_command_interface.py
  example_prompts.md
  test_movements.md
```

## Future extension: AI welding and vision

The later/future idea was to add vision and welding intelligence.

Long-term workflow:

```text
Camera sees part or weld joint
AI/vision system identifies possible weld area
Robot moves near the weld
System creates preliminary weld path
Programmer reviews and adjusts the path
Final program is approved manually
```

This was not meant to fully replace a programmer. The better goal was:

```text
AI creates a first-pass weld plan so the programmer only has to review, correct, and approve it.
```

Future deliverables:

```text
/05_vision_welding_future
  camera_detection_tests/
  weld_joint_detection.py
  preliminary_path_planning.md
  operator_review_workflow.md
```

## Overall project structure

```text
robot-ai-automation-project/
│
├── 01_parametric_cad/
│   ├── robot_config.yaml
│   ├── cad_generator.py
│   ├── models/
│   └── design_notes.md
│
├── 02_motor_sizing/
│   ├── torque_calculator.py
│   ├── motor_sizing.xlsx
│   └── selected_components.md
│
├── 03_ros2_gazebo/
│   ├── robot_description/
│   ├── robot_control/
│   ├── robot_bringup/
│   └── gazebo_worlds/
│
├── 04_ai_agent_control/
│   ├── agent_tools/
│   ├── command_interface.py
│   └── movement_tests.md
│
└── 05_future_welding_vision/
    ├── vision_tests/
    ├── weld_detection/
    └── path_planning/
```

## Simple version

The plan was:

| Stage  | Main goal                                                              |
| ------ | ---------------------------------------------------------------------- |
| 1      | Build a fully parametric robot CAD model                               |
| 2      | Calculate worst-case torque and size motors                            |
| 3      | Convert the robot to URDF and simulate it in ROS 2/Gazebo              |
| 4      | Let an AI agent control the robot through safe movement tools          |
| Future | Add camera vision so AI can identify welds and create rough weld paths |

The strongest version of the project is that it becomes a **full robotics engineering pipeline**:

```text
Parametric CAD → motor sizing → URDF → ROS 2/Gazebo → AI control → future welding vision
```
