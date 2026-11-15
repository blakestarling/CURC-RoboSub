# Columbia RoboSub

<p align="left">
  <img src="./assets/curc-logo.svg" alt="CURC — Columbia University Robotics Club" width="350" height="72">
</p>

CURC is a student organization of Columbia University that competes in multiple robotics competitions. Learn more about the team and our projects on our [website](https://columbiarobotics.club/).

This repository contains all the code needed to run and test the RoboSub team's AUVs ([our website](https://auv.columbiarobotics.club/)). It is built on the Jazzy Jalisco distrobution of [ROS 2](https://www.ros.org/).

## System Flow
- Orange → sensor
- Blue → package
- Yellow → intermediary hardware
- Green → actuators

```mermaid
%%{init: {"theme": "base", "flowchart": {"curve": "basis", "nodeSpacing": 35, "rankSpacing": 40}, "themeVariables": {"lineColor": "#AFAFAF", "textColor": "#EDEDED", "clusterBkg": "transparent", "clusterBorder": "transparent"}}}%%
flowchart TD
    subgraph Sensors["SENSORS"]
        Altimeter("Altimeter"):::sensor
        Camera("Camera"):::sensor
        DepthSensor("Depth Sensor"):::sensor
        IMU("IMU"):::sensor
    end

    Images(["Images"]):::data
    Altitude(["Altitude"]):::data
    Depth(["Depth"]):::data
    InertialData(["Inertial data"]):::data

    subgraph Onboard["SOFTWARE"]
        SensorInterfaces("Sensor Interfaces"):::software
        StateEstimation("State Estimation"):::software
        CameraDriver("Camera Driver"):::software
        ComputerVision("Computer Vision"):::software

        Detections(["Detections"]):::data
        PlanningState(["Estimated state"]):::data
        TaskPlanning("Task Planning"):::software

        Telemetry(["Attitude / telemetry"]):::data
        DesiredMotion(["Desired motion"]):::data
        ControlState(["Estimated state"]):::data
        GuidanceControl("Guidance & Control"):::software

        MotionCommands(["Motion commands"]):::data
        MAVROS("Pixhawk Communication<br/>MAVROS"):::software
    end

    MAVLinkData(["MAVLink"]):::data
    Pixhawk("Pixhawk 6C<br/>ArduSub"):::controller
    ServoOutputs(["Servo outputs"]):::data
    MotorOutputs(["Motor outputs"]):::data

    subgraph ControlledHardware["HARDWARE"]
        Servos("Servos"):::hardware
        Thrusters("Thrusters"):::hardware
    end

    Camera --- Images --> CameraDriver
    Altimeter --- Altitude --> SensorInterfaces
    DepthSensor --- Depth --> SensorInterfaces
    IMU --- InertialData --> SensorInterfaces

    CameraDriver --> ComputerVision
    SensorInterfaces --> StateEstimation
    ComputerVision --- Detections --> TaskPlanning
    StateEstimation --- PlanningState --> TaskPlanning
    StateEstimation --- ControlState --> GuidanceControl
    TaskPlanning --- DesiredMotion --> GuidanceControl
    GuidanceControl --- MotionCommands --> MAVROS
    MAVROS --- Telemetry --> StateEstimation

    MAVROS <--> MAVLinkData <--> Pixhawk
    Pixhawk --- ServoOutputs --> Servos
    Pixhawk --- MotorOutputs --> Thrusters

    classDef sensor fill:#150A02,stroke:#F2B66D,stroke-width:3px,color:#EDEDED,font-weight:bold
    classDef software fill:#000212,stroke:#7399FF,stroke-width:3px,color:#EDEDED,font-weight:bold
    classDef controller fill:#141101,stroke:#F8E77D,stroke-width:3px,color:#EDEDED,font-weight:bold
    classDef hardware fill:#010C02,stroke:#72DB86,stroke-width:3px,color:#EDEDED,font-weight:bold
    classDef data fill:#000E1A,stroke:#1A3E5F,stroke-width:1.5px,color:#99CEFF,font-weight:bold

    style Sensors fill:transparent,stroke:none,color:#EDEDED
    style Onboard fill:transparent,stroke:none,color:#EDEDED
    style ControlledHardware fill:transparent,stroke:none,color:#EDEDED
```

## Setup

### Build Packages

### Run Code

## Visualization and Debugging

## Testing and Code Quality
