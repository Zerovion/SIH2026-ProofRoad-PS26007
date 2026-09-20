# Prototype Design: Safe and Efficient Operation of Mine Vehicles in Fog and Low-Visibility Conditions 
**(Problem Statement: SIH26007)**

## 🚧 Overview & The Problem with Existing Solutions
In open-cast iron ore mines, standard navigation and safety systems completely fail during heavy fog and dust storms:
- **LiDAR** gets blinded by dense particulate matter (fog, heavy dust).
- **GPS** loses signal deep inside terraced open-cast pits.
- **Magnetic Compasses** are rendered useless by the highly magnetic iron ore deposits.

**The Solution:** A novel, 3-layer architecture designed specifically for extreme visibility conditions and highly magnetic, disconnected environments.

---

## ⚙️ Layer 1: Hardware (The Senses)
Instead of relying on optics, this prototype uses sensors that physically penetrate environmental noise.

### 1. 77GHz mmWave Radar (e.g., TI AWR1642)
- **Why:** Unlike LiDAR, millimeter-wave radar slices through thick fog, heavy rain, and iron dust.
- **Function:** Accurately measures distance, velocity, and angle of large obstacles (other trucks, boulders) up to 200 meters away in zero visibility.

### 2. Thermal Imaging (e.g., FLIR Lepton)
- **Why:** Radar handles large machinery, but struggles with organic matter. 
- **Function:** Detects the heat signatures of human workers, small animals, or overheated vehicle components that are obscured by fog.

### 3. Ultra-Wideband (UWB) Mesh (e.g., DWM1000)
- **Why:** GPS fails in deep pits.
- **Function:** Install UWB tags on all vehicles. UWB creates a localized, highly precise Vehicle-to-Vehicle (V2V) network, calculating the exact distance between trucks down to the centimeter without needing satellites.

### 4. Haptic Feedback Seat
- **Why:** In loud, stressful mine environments, audio beeps are often ignored or drowned out. 
- **Function:** Wire vibration motors to the driver's seat. If a truck approaches dangerously from the blind left side, the left side of the seat vibrates, providing immediate, instinctual alerts.

---

## 🧠 Layer 2: AI (The Brain)
The AI must run entirely locally on the edge, as mines do not have reliable cloud connectivity.

### 1. Edge Processing (NVIDIA Jetson Nano/Orin)
- **Function:** Acts as the central brain inside the truck to process sensor data in real-time with zero latency. No internet required.

### 2. Sensor Fusion (Early Fusion)
- **Function:** The AI model overlays bounding boxes from the Thermal camera onto the 3D point-cloud data from the mmWave radar. This creates a foolproof detection system—if the radar misses a pedestrian, the thermal camera catches them.

### 3. Predictive Anti-Slip Trajectory Algorithm
- **Why:** Iron ore pits get incredibly slippery when wet or foggy.
- **Function:** The AI doesn't just trigger an alert based on distance; it calculates the *dynamic stopping distance* based on the vehicle's payload weight, current speed, and weather conditions, triggering early warnings before the physical point of no return.

---

## 💻 Layer 3: Software (The Action)
The software must keep the driver's eyes on the road and provide mine operators with full visibility.

### 1. AR Head-Up Display (HUD)
- **Function:** Project the UI directly onto the windshield or a transparent OLED panel on the dashboard. The software renders "ghost outlines" (wireframes) of vehicles and road edges hidden by fog, using the fused radar/UWB data.

### 2. Decentralized LoRaWAN Control Dashboard
- **Function:** Build a local web server (Node.js/Python Flask) that broadcasts the location of all UWB-tagged vehicles over a low-frequency LoRaWAN network. 
- **Benefit:** The control room gets a live "digital twin" of the mine layout without needing a 4G/5G/Internet connection.

### 3. Driver Fatigue Monitoring
- **Why:** Operating heavy machinery in zero-visibility is highly fatiguing.
- **Function:** Run a lightweight computer vision script inside the cabin to track the driver's blinking rate, pupil dilation, and head position. The system triggers in-cabin alarms and alerts the control room if microsleep is detected.

---

## 🏆 Why this wins the Hackathon (The USP)
- **100% Offline Capable:** Does not rely on cloud computing or standard GPS.
- **Immune to Environment:** Radar and Thermal ignore fog/dust; UWB ignores magnetic interference from iron ore.
- **Driver-Centric:** AR HUD and Haptic feedback reduce cognitive load compared to traditional tablet screens.
