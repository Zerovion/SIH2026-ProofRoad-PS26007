# FogNet — Predictive Fog-Risk Network for Mine Haul Roads
### SIH26007: Safe and Efficient Operation of Mine Vehicles in Fog and Low-Visibility Conditions (Ministry of Steel / NMDC)

---

## The core idea

Every existing solution — commercial radar systems (OndoSense, YUWEI), thermal-camera rigs, and our own earlier camera+dehazing prototype — is **reactive**: they only tell a truck about fog it has already driven into.

**FogNet flips this to predictive.** Instead of sensing fog at the truck, it senses fog *forming* at fixed points across the haul road and warns trucks and dispatch **10–20 minutes before** they reach it — a genuinely different category of solution, not a cheaper version of radar or thermal.

The original vision pipeline (camera → dehazing → YOLOv8n detection) is **not discarded** — it becomes the onboard last-line-of-defense reactive layer, running in parallel with the new predictive network.

---

## Architecture

```
FogNodes (×4–6, distributed along haul road)
   → LoRa mesh (ESP32 + LoRa, no cellular needed)
      → Fog risk prediction (time-series trend + spatial interpolation)
         → Dispatch dashboard (live haul-road risk map)
            → Truck alert (warned before fog is reached)

                                        Onboard vision (dehaze + YOLOv8n + ultrasonic — Tier 1 reactive backup)
                                        → merges into Truck alert
```

### 1. Hardware layer — Electrical + ENTC

| Component | Role |
|---|---|
| IR LED + phototransistor pair | Measures light scattering over a fixed short path — same working principle as a real industrial transmissometer (the instrument actual visibility sensors use), built for ~₹100–200 instead of lakhs |
| Temp + humidity sensor (DHT22/BME280) | Fog forms when air temperature closes in on dew point — real meteorological physics, not a guess |
| ESP32 + LoRa module (×4–6 nodes) | Long-range (multi-km), low-power mesh — no cellular dependency, which mine sites often lack. This is genuinely ENTC's core domain, giving that team member a central technical role |
| Onboard camera + ultrasonic (retained from original design) | Reactive backup for the moment a truck is already inside fog |

Nodes are placed at junctions, blind curves, and low-lying points where fog pools first.

### 2. AI layer — 3 AI/ML members

1. Convert raw scatter reading at each node into a calibrated visibility-distance estimate.
2. Track trend per node (rising humidity + narrowing temp/dewpoint gap + falling scatter = fog incoming) to forecast onset ~10–20 minutes ahead.
3. Spatially interpolate across nodes into a live fog-risk map of the haul road.
4. Retain dehazing (Dark Channel Prior) + YOLOv8n detection as the onboard reactive layer, now reframed as backup rather than the centerpiece.

**Honest scope note:** with only hours of hackathon data, don't claim a trained ML model has learned real fog dynamics — use a physics-informed trend/threshold model you can actually validate. This is more defensible under judge questioning than an oversold "AI predicts the weather" claim.

### 3. Software layer

- **Dispatch dashboard:** color-coded live map of haul-road segments (green/amber/red) with current + predicted risk, so a controller can hold or reroute a truck before it reaches a bad stretch.
- **Truck alert:** simple buzzer/LED/haptic alert pushed via LoRa *ahead of arrival* — a capability no single-truck radar/thermal system can offer, since it has no way to know about a location it hasn't physically reached.
- **Logging:** compare predicted vs. actual fog onset per node over time to improve the model — turns the system into a growing dataset, not just a live demo.

---

## Why this is genuinely new

- Existing mine safety tech (radar, thermal, GNSS-based proximity) operates at the **single-vehicle level** — a crowded, vendor-dominated space (OndoSense, YUWEI, published mmWave-radar mine studies).
- Predictive, network-level fog-risk mapping for haul roads does not appear in existing commercial products or research — the general *method* (distributed IoT sensors + predictive ML for mine hazards) is proven for other risks like slope failure and gas concentration, but not yet applied to fog specifically.
- It gives **lead time**, which no reactive system — including our own original prototype — can provide.

## Honest caveats to state upfront (don't wait for judges to find them)

- The LED-phototransistor scatter sensor is a simplified visibility proxy; ambient light and dust-vs-fog discrimination need real calibration — it's not lab-grade.
- Fog prediction accuracy with hackathon-scale data is not validated; pitch it as a physics-informed early-warning heuristic, not a proven forecasting model.
- FogNet complements on-truck sensing rather than replacing it — position it as "the layer that's missing," not "the layer that makes radar obsolete."

---

## Build strategy (36-hour window)

- **Tier 1 (guaranteed floor):** Original camera → dehazing → YOLOv8n → dashboard alert (already validated as buildable).
- **Tier 2 (the new differentiator):** 2–3 FogNodes (scaled down from 4–6 for time) with scatter + temp/humidity sensors, LoRa mesh to a central dashboard, simple trend-based risk prediction, and a truck-side LoRa receiver triggering an early alert.

Demo plan: simulate fog at 2–3 physically separated points (fog machine / humidifier at each), show the dashboard lighting up a risk zone *before* the "truck" (camera rig) physically reaches it — this before/after contrast against the old reactive-only demo is the strongest way to show judges the actual innovation.
