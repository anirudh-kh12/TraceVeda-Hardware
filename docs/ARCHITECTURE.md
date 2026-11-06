# System Architecture

## Purpose

The TraceVeda hardware layer collects environmental and handling telemetry at two stages: stationary storage and transportation.

## System overview

```mermaid
flowchart TB
  subgraph S["Storage Node — ESP32"]
    S1["DHT22: temperature + humidity"]
    S2["BH1750: illuminance"]
    S3["Limit switch: door state"]
    S4["20×4 I²C LCD"]
    S5["Red LED"]
    SC["Storage Node firmware"]
    S1 --> SC
    S2 --> SC
    S3 --> SC
    SC --> S4
    SC --> S5
  end

  subgraph T["Truck Node — ESP32"]
    T1["DHT22: temperature + humidity"]
    T2["BH1750: illuminance"]
    T3["Limit switch: door state"]
    T4["NEO-6M GPS"]
    T5["20 kg load cell + HX711"]
    T6["16×2 I²C LCD"]
    T7["Red LED"]
    TC["Truck Node firmware"]
    T1 --> TC
    T2 --> TC
    T3 --> TC
    T4 --> TC
    T5 --> TC
    TC --> T6
    TC --> T7
  end

  SC -->|"Wi-Fi telemetry"| API["TraceVeda backend"]
  TC -->|"Wi-Fi telemetry"| API
  API -->|"response including red_led state"| SC
  API -->|"response including red_led state"| TC
```

## Storage Node

The Storage Node monitors:
- Temperature and relative humidity using DHT22.
- Ambient illuminance using BH1750.
- Door state using a limit switch.

It displays information on a 20×4 I²C LCD and controls a red LED using the backend response.

## Truck Node

The Truck Node monitors:
- Temperature and relative humidity using DHT22.
- Ambient illuminance using BH1750.
- Compartment-door state using a limit switch.
- GPS position using a NEO-6M receiver.
- Payload weight using a 20 kg load cell and HX711 amplifier.

It displays information on a 16×2 I²C LCD and controls a red LED using the backend response.

## Backend integration

Both nodes were integrated with the backend. Their final firmware sends telemetry to:

`POST /api/iot/readings`

The firmware also receives and processes backend responses. The red LED output is controlled using the `red_led` field described in the source comments and response-handling code.

The API base URL and device key are configuration values and are not documented here as live credentials.
