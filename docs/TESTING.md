# Testing and Validation

## Component test sketches retained

| Test | Purpose |
|---|---|
| ESP32 test | Basic board/serial test |
| DHT22 test | Temperature and humidity readings |
| BH1750 test | Ambient illuminance readings |
| LCD 16×4 test | Character LCD testing |
| LCD 20×4 test | Character LCD testing |
| GPS test | GPS serial data testing |
| Limit switch test | Digital switch input |
| Load cell + HX711 test | Raw readings and calibration procedure |

The MPU-6500 experiment is stored separately under `firmware/Development/Truck_Node/` because it was removed from the final design.

## Integration status

The user confirms that both Storage Node and Truck Node were fully integrated with the backend, including sending data and receiving backend responses.

The final firmware source also contains:
- HTTP telemetry submission to `/api/iot/readings`.
- Backend response parsing for the `red_led` state.
- RAM queue and LittleFS-related offline handling.

## What is not claimed

The supplied archive does not contain a complete structured test-results report with dates, measured accuracy, repeatability, end-to-end latency, or pass rates. This document therefore does not claim numerical performance.

The final physical prototype is no longer available for inspection because it was dismantled and the borrowed IoT-lab components were returned.

## Known load-cell limitation

The Truck Node's 20 kg load-cell/HX711 path was integrated, but the calibration/zeroing issue remained unresolved. The repository records the calibration constant found in the firmware, but does not claim accurate or validated weight readings.

## Recommended format for any future test evidence

| Field | Record |
|---|---|
| Test ID | Unique identifier |
| Date | Actual test date |
| Hardware | Board and module variant |
| Firmware | Sketch and revision |
| Procedure | Steps performed |
| Expected result | Functional expectation |
| Actual observation | What was measured or observed |
| Status | Pass / fail / inconclusive |
| Evidence | Original photograph, serial log or measurement |
| Notes | Conditions and limitations |

Do not fill this table with reconstructed or estimated results. Use only retained notes, logs or repeatable tests.
