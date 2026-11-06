# Hardware System Architecture

## Purpose

The TraceVeda hardware layer gathers physical-condition and handling telemetry from two points in the herbal supply chain: a stationary storage environment and a moving transport vehicle.

## System-level view

```mermaid
flowchart TB
    subgraph S["Storage Node — ESP32"]
      S1["DHT22\nTemperature + humidity"]
      S2["BH1750\nIlluminance"]
      S3["Limit switch\nDoor state"]
      S4["20×4 I²C LCD"]
      S5["Red LED"]
      SC["ESP32 firmware"]
      S1 --> SC
      S2 --> SC
      S3 --> SC
      SC --> S4
      SC --> S5
    end

    subgraph T["Truck Node — ESP32"]
      T1["DHT22\nTemperature + humidity"]
      T2["BH1750\nIlluminance"]
      T3["Limit switch\nDoor state"]
      T4["GPS receiver\nPosition"]
      T5["Load cell + HX711\nWeight"]
      T6["16×2 I²C LCD"]
      T7["Red LED"]
      TC["ESP32 firmware"]
      T1 --> TC
      T2 --> TC
      T3 --> TC
      T4 --> TC
      T5 --> TC
      TC --> T6
      TC --> T7
    end

    SC -->|"Wi-Fi / HTTP(S) API\nPOST /api/iot/readings"| API["TraceVeda backend"]
    TC -->|"Wi-Fi / HTTP(S) API\nPOST /api/iot/readings"| API
    API --> DB["Backend data store"]
    API -.->|"red_led state in response"| SC
    API -.->|"red_led state in response"| TC
```

The firmware comments describe HTTPS/API telemetry and device-key authentication. The actual base URL and credentials are intentionally omitted from this portfolio copy.

## Node responsibilities

### Storage Node

- Reads temperature and relative humidity from DHT22.
- Reads illuminance from BH1750 over I²C.
- Reads a digital limit switch as the storage-door state.
- Displays status/readings on a 20×4 I²C LCD.
- Drives a red LED using the state received from the backend.
- Queues telemetry and uses LittleFS for offline backlog handling.

### Truck Node

- Reads temperature and relative humidity from DHT22.
- Reads illuminance from BH1750 over I²C.
- Reads a compartment-door limit switch.
- Receives GPS data over UART2 and parses NMEA data using TinyGPSPlus.
- Reads a load cell through an HX711 amplifier.
- Displays rotating status/readout screens on a 16×2 I²C LCD.
- Queues telemetry and uses LittleFS for offline backlog handling.
- Drives a red LED using backend response state.

## Boundary of the hardware contribution

The ESP32 nodes acquire, package, queue, and transmit device readings. Backend persistence, threshold policy, batch workflows, dashboards, and the rest of the supply-chain platform belong to the wider TraceVeda software system.
