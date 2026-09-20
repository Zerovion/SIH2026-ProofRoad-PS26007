# FOGSHIELD
## Smart Mine Vehicle Safety System for Fog and Low-Visibility Conditions

**SIH Problem Statement:** SIH26007  
**Problem:** Safe and Efficient Operation of Mine Vehicles in Fog and Low-Visibility Conditions in Open Cast Iron Ore Mines

---

## 1. Executive Summary

FOGSHIELD is a practical, low-cost prototype for improving the safety and continuity of mine vehicle operations during fog and low-visibility conditions.

The system does not try to make the driver literally "see through fog." Instead, it makes the vehicle intelligent enough to understand its surroundings using multiple sensing technologies, predict hazardous situations, calculate a dynamically safe operating speed, and communicate the risk to the driver and mine control room.

The architecture has three layers:

1. **Hardware Layer** - Radar, camera, GPS, IMU, environmental sensing, V2V communication and driver alerts.
2. **AI Layer** - Fog estimation, object detection, sensor fusion, collision prediction, risk assessment and dynamic safe-speed calculation.
3. **Software Layer** - Driver safety interface, fleet monitoring, digital mine map, alerts, safety-event recording and analytics.

The central innovation is the **Dynamic Safety Envelope**: instead of using a fixed warning distance, FOGSHIELD continuously changes the vehicle's safety boundary according to visibility, speed, road geometry, traffic, object movement and stopping requirements.

---

# 2. Core Concept

## The problem with conventional systems

A simple system may do:

```text
Camera -> Detect Truck -> Warning
```

FOGSHIELD instead does:

```text
Camera
Radar
GPS
IMU
V2V
Environment
   |
   v
Sensor Fusion
   |
   v
Environment Model
   |
   +--> Fog/Visibility Estimation
   +--> Object Tracking
   +--> Collision Prediction
   +--> Road Risk
   |
   v
Dynamic Safety Envelope
   |
   v
Safe-Speed + Risk Decision
   |
   +--> Driver Warning
   +--> Haptic/Audio Alert
   +--> Fleet Dashboard
   +--> Safety Event Record
```

The system is therefore designed as a **decision-support and predictive safety system**, not merely an obstacle detector.

---

# 3. Design Philosophy

Every feature must answer at least one of these questions:

- Does it reduce collision risk?
- Does it help the driver operate safely in fog?
- Does it identify a hazard that vision alone may miss?
- Does it help the control room manage vehicle movement?
- Does it create useful safety data?
- Does it support future deployment in an actual mine?

If a feature does not provide a clear safety or operational benefit, it should not be included.

---

# 4. Three-Layer Architecture

```text
+----------------------------------------------------------+
|                     LAYER 3: SOFTWARE                    |
|                                                          |
| Driver Safety UI | Fleet Dashboard | Digital Mine Map   |
| Alerts | Safety Black Box | Analytics | Event History   |
+----------------------------^-----------------------------+
                             |
+----------------------------|-----------------------------+
|                       LAYER 2: AI                        |
|                                                          |
| Fog Estimation | Object Detection | Sensor Fusion       |
| Object Tracking | TTC | Collision Prediction            |
| Dynamic Safe-Speed Engine | Risk Classification          |
+----------------------------^-----------------------------+
                             |
+----------------------------|-----------------------------+
|                     LAYER 1: HARDWARE                    |
|                                                          |
| Radar | Camera | GPS | IMU | V2V | Environment Sensors  |
| Edge Controller | Buzzer | Vibration | LEDs             |
+----------------------------------------------------------+
```

---

# 5. Layer 1 - Hardware

## 5.1 Forward Radar

### Purpose

The radar detects physical objects even when the camera's visibility is severely degraded.

### Prototype choice

**24 GHz multi-target radar, such as RD-03D**

### Detects

- Vehicle
- Large object
- Approximate distance
- Relative movement
- Approaching/receding objects

### Why it matters

Fog can significantly reduce optical visibility. Radar provides a second sensing modality that does not depend on normal visual contrast.

---

## 5.2 Camera

### Purpose

The camera provides visual information and supports:

- Object classification
- Road understanding
- Fog estimation
- Lane/road-edge understanding
- Visual confirmation of radar detections

The camera is deliberately not treated as the only safety sensor.

### Sensor roles

```text
Camera -> What is the object?
Radar  -> Where is it and how is it moving?
GPS    -> Where are we?
IMU    -> How are we moving?
V2V    -> Which connected vehicle is nearby?
AI     -> What does all of this mean?
```

---

## 5.3 GPS

### Purpose

GPS provides vehicle position and supports:

- Vehicle tracking
- Road-segment identification
- Vehicle-to-vehicle positioning
- Risk-zone identification
- Digital mine map

### Prototype

NEO-6M GPS modules.

### Production version

RTK/DGPS or an industrial-grade GNSS solution can be used for improved accuracy.

---

## 5.4 IMU

### Prototype

MPU6050.

### Purpose

Measures:

- Acceleration
- Rotation
- Vehicle movement
- Directional changes
- Sudden movement

It helps determine how the vehicle is behaving rather than relying only on GPS.

---

## 5.5 V2V Communication

### Prototype technology

ESP32 + LoRa modules.

Each connected vehicle can broadcast:

```text
Vehicle ID
GPS position
Speed
Direction
Timestamp
Risk state
```

Example:

```text
Vehicle: D12
Distance: 24 m
Direction: Ahead
Speed: 11 km/h
Status: CAUTION
```

The receiving vehicle can then display a virtual representation of D12 even when D12 is difficult or impossible to visually see through fog.

---

# 6. Ghost Vehicle Awareness

One of the strongest demonstration features is **Ghost Vehicle Awareness**.

Suppose a truck is hidden by fog.

The camera may produce:

```text
Low visual confidence
```

But radar and V2V can still provide:

```text
Radar:
Object at 18.4 m

V2V:
Vehicle D12 at 18.1 m

GPS:
Position consistent

AI:
High-confidence vehicle ahead
```

The driver interface can therefore show:

```text
GHOST VEHICLE DETECTED

D12
18.2 m ahead
11 km/h
Closing
```

The purpose is not to create an artificial visual gimmick. It is to convert otherwise invisible or poorly visible vehicles into actionable safety information.

---

# 7. Road-Edge and Clearance Monitoring

Mine vehicle hazards include more than other vehicles.

Important hazards include:

- Road edge
- Berm
- Sharp curve
- Slope
- Intersections
- Excavation edge
- Narrow haul-road sections

The prototype can use side/angled sensing where practical to estimate clearance.

Example:

```text
Left clearance: 2.1 m
Right clearance: 4.8 m
```

If the vehicle approaches an unsafe edge:

```text
EDGE DANGER
Reduce speed / Correct trajectory
```

---

# 8. Environmental Sensing

A DHT22 or equivalent environmental sensor can provide:

- Temperature
- Relative humidity

These readings can support environmental context and fog estimation.

The prototype can also include an ambient light sensor if available.

---

# 9. Driver Warning Hardware

FOGSHIELD should not depend only on a screen.

It uses:

### Visual

- Status display
- Warning indicators
- Risk state

### Audio

- Buzzer
- Escalating warning pattern

### Haptic

- Vibration motor

Example:

```text
LOW RISK
Single short indication

CAUTION
Repeated indication

HIGH RISK
Rapid warning

CRITICAL
Continuous warning
```

---

# 10. Layer 2 - AI

The AI layer is the intelligence core of FOGSHIELD.

It consists of multiple focused engines rather than one oversized AI model.

---

# 11. AI Engine 1 - Fog Severity Estimator

The system estimates the effective visibility condition.

Possible states:

```text
NORMAL
   |
REDUCED
   |
POOR
   |
CRITICAL
```

Camera-based features may include:

- Image contrast
- Edge density
- Brightness
- Visibility of known objects
- Atmospheric scattering indicators

Environmental context may include:

- Humidity
- Temperature
- Ambient light

The output is an estimated visibility value and severity class.

Example:

```text
Visibility: 6.1 m
Fog State: CRITICAL
```

The visibility estimate becomes an input to the safety engine.

---

# 12. AI Engine 2 - Object Detection

A lightweight computer-vision model such as YOLO can identify objects such as:

- Mine truck
- Person
- Large obstacle
- Road-related hazard

The AI model should not make the final safety decision alone.

Example:

```text
Camera:
Truck detected
Confidence: 0.84
```

The radar may simultaneously report:

```text
Object:
Distance: 16.7 m
Relative speed: -4.2 m/s
```

These measurements are then fused.

---

# 13. AI Engine 3 - Sensor Fusion

This is one of the most important components.

Instead of trusting a single sensor:

```text
Camera
Radar
GPS
IMU
V2V
Environment
   |
   v
Sensor Fusion
   |
   v
Unified Object/Environment Model
```

Example:

```text
Camera confidence: 42%
Radar confidence: 96%
V2V confidence: 91%

Final fused confidence: 94%
```

If camera confidence falls heavily in dense fog, radar and V2V can still contribute to the decision.

This is a key reason for using heterogeneous sensors.

---

# 14. AI Engine 4 - Object Tracking

A single detection is not enough.

The system should track an object over time.

For each tracked vehicle:

```text
Position
Distance
Speed
Direction
Relative velocity
Trajectory
Time history
```

This allows FOGSHIELD to distinguish between:

```text
Object is stationary
```

and

```text
Object is approaching
```

---

# 15. AI Engine 5 - Collision Prediction

The system predicts whether the current movement could lead to a collision.

A key metric is:

## Time To Collision (TTC)

Simplified concept:

```text
TTC = Distance / Closing Speed
```

Example:

```text
Distance = 22 m
Closing speed = 11 m/s

TTC = 2 seconds
```

The system can classify:

```text
LOW
MEDIUM
HIGH
CRITICAL
```

TTC should be combined with additional context rather than used as the only decision variable.

---

# 16. AI Engine 6 - Dynamic Safe-Speed Engine

This is the signature decision feature.

Instead of simply saying:

> "Obstacle detected"

FOGSHIELD estimates:

> **What speed is safe under the current conditions?**

Inputs:

```text
Effective visibility
Current vehicle speed
Object distance
Relative speed
TTC
Road curvature
Road width
Road-edge clearance
Traffic density
Nearby vehicle state
Vehicle stopping capability
Road risk
```

Output:

```text
Recommended safe speed
Risk level
Required action
```

Example:

```text
Visibility: 7 m
Curve: Medium
Traffic: Medium
Object: 18 m
TTC: 3.1 s

Recommended speed: 8 km/h
Risk: HIGH
```

If conditions become critical:

```text
Recommended speed: 0 km/h
Action: STOP
```

---

# 17. Dynamic Safety Envelope

## Core innovation

FOGSHIELD continuously creates a virtual safety boundary around the vehicle.

Unlike a fixed warning radius, the envelope changes dynamically.

### Factors affecting the envelope

- Speed
- Visibility
- Stopping distance
- Road curvature
- Vehicle trajectory
- Nearby traffic
- Road-edge clearance
- Object movement
- Risk level

Conceptually:

```text
Good visibility:

        +----------------+
        |                |
        |     DUMPER     |
        |                |
        +----------------+

Dense fog:

       +------------------+
      /                    \
     |       DUMPER         |
      \                    /
       +------------------+
```

The actual mathematical shape is determined by the vehicle state and road geometry.

---

# 18. Why the Dynamic Safety Envelope Matters

A fixed 10 m warning zone is not equally safe at every speed or visibility level.

For example:

```text
10 m at 5 km/h
```

may provide substantially more response time than:

```text
10 m at 25 km/h
```

FOGSHIELD therefore adapts the safety boundary instead of using one fixed distance.

This converts raw sensing into a safety decision.

---

# 19. Predictive Road Intelligence

The mine haul road is divided into logical segments.

Example:

```text
A -------- B -------- C
                    /
                   D
                   |
                   E
```

Each road segment can store:

```text
Road width
Curve severity
Gradient
Visibility history
Traffic density
Typical vehicle speed
Known hazard areas
Near-miss history
```

The system can then warn the driver before entering a known high-risk section.

Example:

```text
HIGH-RISK ZONE AHEAD

Zone C
Visibility historically poor

Recommended speed: 8 km/h
```

This makes the system predictive rather than purely reactive.

---

# 20. Risk Scoring

FOGSHIELD can combine the different safety inputs into a risk score.

Conceptually:

```text
Risk =
    Visibility Risk
  + Collision Risk
  + Speed Risk
  + Road Geometry Risk
  + Edge Clearance Risk
  + Traffic Risk
```

The exact weighting should be calibrated during testing.

Output:

```text
0-25    LOW
26-50   MODERATE
51-75   HIGH
76-100  CRITICAL
```

These thresholds should be treated as prototype values and validated before any real deployment.

---

# 21. Layer 3 - Software

The software should have three major interfaces.

---

# 22. Driver Safety Interface

The driver interface must be simple.

It should prioritize:

1. Safe speed
2. Immediate hazard
3. Distance
4. Risk state
5. Visibility

Example:

```text
+--------------------------------+
|          FOGSHIELD             |
|                                |
|        SAFE SPEED              |
|             08                 |
|            km/h                |
|                                |
|       VEHICLE AHEAD            |
|            21.4 m              |
|                                |
|        TTC: 3.2 sec            |
|                                |
|        VISIBILITY              |
|           6.1 m                |
|                                |
|       STATUS: CAUTION          |
+--------------------------------+
```

The driver should not have to interpret complex graphs while operating the vehicle.

---

# 23. Fleet Control Dashboard

The mine control room gets a fleet-level view.

Example:

```text
             MINE OVERVIEW

       +-----------------------+
       |                       |
       | D01 ---->             |
       |             D07       |
       |                       |
       |        HIGH RISK      |
       |          ZONE C       |
       |                       |
       | D04 <----             |
       +-----------------------+

Fleet:
18 Vehicles

SAFE       14
CAUTION     3
CRITICAL    1

Visibility:
Zone A     42 m
Zone B     17 m
Zone C      5 m
```

The dashboard can show:

- Vehicle locations
- Risk states
- Visibility by zone
- Active alerts
- Traffic density
- Road risk
- Safety events

---

# 24. Digital Mine Map

The digital map should be functional, not decorative.

Represent:

```text
Mine
 |
 +-- Haul Road
 |    +-- Zone A
 |    +-- Zone B
 |    +-- Zone C
 |
 +-- Vehicles
 |
 +-- Risk Zones
 |
 +-- Environmental State
```

Vehicle locations update using GPS.

Risk zones can change based on the current state.

---

# 25. Safety Black Box

Every major safety event is recorded.

Example:

```text
TIME: 12:42:31
VEHICLE: D07

Visibility: 4.8 m
Vehicle speed: 17 km/h

Object detected: D12
Distance: 19 m

TTC: 2.1 sec
AI Risk: HIGH

Recommended speed: 7 km/h
Driver response: Reduced to 8 km/h
```

This creates a safety history that can support:

- Near-miss analysis
- Driver training
- Road-risk analysis
- Safety audits
- Future model improvement

---

# 26. Proposed Technology Stack

## Hardware

```text
ESP32
RD-03D / equivalent radar
ESP32-CAM
NEO-6M GPS
MPU6050
SX1278 LoRa
DHT22
Buzzer
Vibration motor
LEDs
Battery
DC-DC converter
```

## AI / Backend

```text
Python
OpenCV
YOLO
FastAPI
MQTT
SQLite
```

## Frontend

```text
React
TypeScript
Tailwind CSS
Leaflet / map visualization
Charts
```

The AI can initially run on an existing laptop instead of requiring a dedicated edge computer.

---

# 27. Prototype Architecture

```text
             MINIATURE MINE VEHICLE

       +----------------------------+
       |                            |
       |         CAMERA             |
       |            |               |
       |       +----v----+          |
       |       |  ESP32  |          |
       |       +----+----+          |
       |            |               |
       |    RADAR --+               |
       |            |               |
       |     GPS ---+               |
       |            |               |
       |     IMU ---+               |
       |            |               |
       |    LoRa ---+               |
       |            |               |
       |   Buzzer/Vibration         |
       +----------------------------+
                    |
                    | Wi-Fi / LoRa
                    v
              AI / BACKEND
                    |
        +-----------+-----------+
        |                       |
        v                       v
 DRIVER UI                CONTROL ROOM
```

---

# 28. Physical Demonstration Setup

Build a miniature open-cast mine haul road.

```text
+-----------------------------------------+
|                FOG AREA                 |
|                                         |
| START ->  TRUCK A           TRUCK B     |
|                                         |
|              CURVE                      |
|                 \                       |
|                  \____ ROAD EDGE        |
|                                         |
+-----------------------------------------+
```

Use controlled artificial fog for demonstration.

Demonstrate at least four scenarios.

---

# 29. Demo Scenario 1 - Hidden Vehicle

Vehicle B is partially hidden by fog.

Camera visibility becomes poor.

Radar detects the vehicle.

V2V confirms it.

Driver interface:

```text
GHOST VEHICLE
D12
14.2 m AHEAD
```

---

# 30. Demo Scenario 2 - Collision Risk

Vehicle A approaches Vehicle B too quickly.

System calculates:

```text
Distance: 22 m
Closing speed: 11 m/s
TTC: 2 sec
```

System responds:

```text
CRITICAL RISK

SAFE SPEED: 0 km/h

STOP
```

Buzzer and vibration activate.

---

# 31. Demo Scenario 3 - Road Edge

Vehicle approaches the road boundary.

System estimates:

```text
Edge clearance:
2.2 m
1.5 m
0.9 m
```

Warning escalates:

```text
EDGE DANGER
REDUCE SPEED
```

---

# 32. Demo Scenario 4 - V2V Ghost Vehicle

Vehicle B is hidden behind a fog screen.

Camera cannot confidently classify it.

V2V reports:

```text
D12
Position
Speed
Direction
```

The system creates a virtual vehicle marker.

This demonstrates that connected vehicles can remain safety-aware even when visual identification becomes difficult.

---

# 33. Prototype Bill of Materials

The target is to keep the complete student prototype close to **₹6,000**.

| Component | Qty | Target Unit Price | Total |
|---|---:|---:|---:|
| ESP32 DevKit | 2 | ₹308 | ₹616 |
| 24 GHz RD-03D Radar | 1 | ₹1,027 | ₹1,027 |
| ESP32-CAM | 1 | ₹499 | ₹499 |
| NEO-6M GPS | 2 | ₹239 | ₹478 |
| SX1278 LoRa | 2 | ₹341 | ₹682 |
| MPU6050 | 1 | ₹137 | ₹137 |
| DHT22 | 1 | ₹115 | ₹115 |
| Buzzer | 1 | ₹30 | ₹30 |
| Vibration Motor | 1 | ₹30 | ₹30 |
| LEDs | 4 | ₹10 | ₹40 |
| Push Buttons | 3 | ₹5 | ₹15 |
| Jumper Wires | 1 set | ₹150 | ₹150 |
| Breadboard | 2 | ₹80 | ₹160 |
| Resistors/Capacitors | 1 set | ₹100 | ₹100 |
| Battery + Holder | 2 | ₹150 | ₹300 |
| DC-DC Converter | 2 | ₹70 | ₹140 |
| Mini Vehicle/Chassis | 1 | ₹500 | ₹500 |
| Fog Demonstration Materials | 1 | ₹200 | ₹200 |
| Mounting/Enclosure Materials | 1 | ₹250 | ₹250 |
| **Estimated Total** | | | **₹5,469** |

### Budget target

**Hardware prototype: approximately ₹5,469**

Keep a practical procurement ceiling of:

**₹6,000**

Prices can vary by seller and time.

---

# 34. How to Spend the ₹6,000 Wisely

Do not purchase an expensive Raspberry Pi, Jetson, thermal camera or industrial radar for the first prototype.

Run the AI/backend on an existing laptop.

### Phase 1 - Core prototype

Target:

**₹3,200-₹3,500**

Build:

- Radar
- Camera
- ESP32
- GPS
- IMU
- Warning system
- AI
- Basic dashboard

### Phase 2 - Differentiation

Target additional:

**₹1,500-₹1,800**

Add:

- Second vehicle
- LoRa V2V
- Second GPS
- Ghost vehicle
- Collision prediction
- Dynamic safety envelope

### Phase 3 - Physical presentation

Target:

**₹500-₹700**

Add:

- Miniature mine road
- Fog chamber
- Vehicle body
- Warning indicators
- Proper sensor mounting

---

# 35. What Should NOT Be Added

Avoid features that sound impressive but do not directly improve the problem.

Do not prioritize:

- Facial recognition
- Driver emotion detection
- Generic chatbot
- Voice assistant
- Decorative AR
- Unnecessary 3D animations
- Automatic braking on the student prototype
- Fully autonomous vehicle claims

The prototype should remain focused on:

```text
SENSE
  ->
UNDERSTAND
  ->
PREDICT
  ->
WARN
  ->
RECORD
```

---

# 36. Existing vs FOGSHIELD

FOGSHIELD should not claim that radar, thermal cameras, GNSS or V2X are individually new technologies. Such technologies already exist in mining and vehicle-safety research.

The differentiation is in the integrated decision layer.

| Conventional approach | FOGSHIELD |
|---|---|
| Detect obstacle | Predict collision |
| Fixed warning distance | Dynamic safety envelope |
| Camera-dependent view | Multi-sensor environment |
| Visible vehicle detection | Ghost vehicle awareness |
| Current position | Position + road risk |
| Fog detection | Visibility-aware speed |
| Warning | Adaptive action recommendation |
| Fleet dashboard | Fleet safety intelligence |
| Accident detection | Near-miss recording |
| Reactive operation | Predictive operation |

The strongest innovation claim is therefore:

> **FOGSHIELD uses a dynamic, visibility-aware safety envelope to continuously determine how safely a mine vehicle can operate under changing fog, traffic, road and vehicle conditions.**

---

# 37. Production Deployment Path

The student prototype is intentionally low-cost.

A future mine deployment could replace prototype components with industrial-grade hardware.

| Prototype | Production Direction |
|---|---|
| RD-03D | Industrial automotive/mining-grade radar |
| ESP32 | Industrial edge controller |
| NEO-6M | RTK/DGPS GNSS |
| ESP32-CAM | Rugged industrial camera |
| LoRa | Industrial V2V/V2X |
| Laptop AI | Rugged edge AI computer |
| Breadboard | Industrial PCB |
| Open electronics | IP-rated enclosure |
| Mini vehicle | Full-scale haul truck |
| Prototype fog model | Real mine environmental validation |

The software and decision architecture can remain conceptually similar while the hardware is upgraded.

---

# 38. Safety and Engineering Boundaries

FOGSHIELD is a prototype and should be presented as a **driver-assistance and safety decision-support system**, not as a certified autonomous driving or emergency-braking system.

Before deployment on real mine vehicles, the following would require engineering validation:

- Sensor accuracy
- Radar performance
- GNSS accuracy
- Environmental robustness
- Dust/water resistance
- Electromagnetic compatibility
- Fail-safe behavior
- Warning latency
- Vehicle stopping distance
- Human-machine interface
- Mine-specific safety regulations
- Industrial certification
- Functional safety requirements

The prototype must never control a real heavy vehicle without appropriate safety engineering and certification.

---

# 39. Key Performance Indicators for the Prototype

Measure the prototype rather than only showing features.

Suggested KPIs:

### Detection

- Object detection rate
- Radar detection distance
- Detection confidence

### Prediction

- TTC estimation
- Collision-risk classification accuracy

### Visibility

- Estimated visibility
- Fog severity classification

### Response

- Warning latency
- Time from detection to alert

### Safety

- False warning rate
- Missed hazard rate
- Safe-speed recommendation consistency

### Communication

- V2V message latency
- Packet reception rate

---

# 40. Suggested Test Matrix

| Scenario | Camera | Radar | V2V | Expected Result |
|---|---|---|---|---|
| Clear road | Good | Active | Active | Normal operation |
| Light fog | Reduced | Active | Active | Reduced safe speed |
| Dense fog | Poor | Active | Active | Strong warning |
| Hidden truck | Poor | Active | Active | Ghost vehicle |
| Fast approach | Any | Active | Active | High/critical risk |
| Road-edge approach | Any | Side sensing | N/A | Edge warning |
| High-risk curve | Any | Active | Active | Predictive slowdown |
| V2V vehicle | Poor | Optional | Active | Virtual vehicle |

---

# 41. Example End-to-End Decision

Suppose:

```text
Visibility = 5 m
Vehicle speed = 18 km/h
Road curve = High
Object distance = 14 m
Closing speed = 6 m/s
Left clearance = 1.2 m
V2V vehicle = Present
```

FOGSHIELD processes:

```text
Fog Engine
    ->
CRITICAL VISIBILITY

Object Tracking
    ->
Vehicle ahead

TTC Engine
    ->
~2.3 seconds

Road Risk
    ->
HIGH

Edge Risk
    ->
HIGH

Dynamic Safety Envelope
    ->
Envelope breached
```

Final decision:

```text
RISK: CRITICAL

SAFE SPEED: 0-5 km/h
ACTION: STOP / IMMEDIATE SPEED REDUCTION

AUDIO: ACTIVE
HAPTIC: ACTIVE
CONTROL ROOM: ALERT
EVENT: RECORDED
```

---

# 42. Main Innovation Stack

The project can be summarized as five connected innovations:

## 1. Multi-Sensor Safety Fusion

Radar + camera + GPS + IMU + V2V.

## 2. Ghost Vehicle Awareness

Connected/radar-detected vehicles remain visible to the system even when fog hides them from the camera.

## 3. Dynamic Safety Envelope

The safety boundary changes according to real-world conditions.

## 4. Visibility-Aware Safe Speed

The system recommends an operating speed rather than merely issuing a generic warning.

## 5. Predictive Mine Road Intelligence

Known high-risk road segments can be identified before the vehicle enters them.

Together these form a complete safety architecture.

---

# 43. Final Product Definition

## FOGSHIELD

### **AI-powered Dynamic Safety Envelope for Mine Vehicles in Fog**

```text
SENSE
Radar + Camera + GPS + IMU + V2V + Environment
                    |
                    v
UNDERSTAND
Fog + Objects + Road + Traffic
                    |
                    v
PREDICT
TTC + Collision Risk + Road Risk
                    |
                    v
DECIDE
Dynamic Safety Envelope
+
Safe Operating Speed
                    |
                    v
ACT
Visual + Audio + Haptic Warning
                    |
                    v
RECORD
Safety Black Box + Fleet Analytics
```

---

# 44. One-Line Pitch

> **FOGSHIELD does not try to remove the fog; it makes the mine vehicle intelligent enough to operate safely within it.**

---

# 45. SIH Presentation Pitch

> **FOGSHIELD is a low-cost AI-powered safety system for open-cast mine vehicles operating under fog and low-visibility conditions. It fuses radar, computer vision, GPS, IMU and vehicle-to-vehicle communication to build a real-time environment model around the vehicle. Its key innovation is a Dynamic Safety Envelope that adapts to visibility, speed, road geometry, vehicle movement and traffic conditions. Instead of simply detecting an obstacle, FOGSHIELD predicts collision risk, recommends a safe operating speed, provides multimodal driver warnings, identifies virtually invisible connected vehicles, and records near-miss events for mine safety analytics.**

---

# 46. Prototype Goal

The prototype should prove one simple statement:

> **A mine vehicle does not need perfect visibility to make a safer movement decision, provided the system can reliably sense, fuse, predict and communicate the surrounding risk.**

This is the core engineering principle behind FOGSHIELD.
