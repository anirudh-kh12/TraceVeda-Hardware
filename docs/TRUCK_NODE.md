# Truck Node

## Overview

The Truck Node is the transport monitoring unit built around an ESP32 development board.

### Final hardware

- ESP32 development board
- DHT22
- BH1750
- NEO-6M GPS receiver
- Limit switch
- 20 kg load cell with HX711 amplifier
- 16×2 I²C LCD
- Red LED

### Final firmware

`firmware/Truck_Node/Truck_Node.ino`

## Firmware-derived pin mapping

| Peripheral signal | ESP32 connection |
|---|---|
| DHT22 data | GPIO 4 |
| BH1750 SDA | GPIO 21 |
| BH1750 SCL | GPIO 22 |
| LCD SDA | GPIO 21 |
| LCD SCL | GPIO 22 |
| GPS TX → ESP32 RX | GPIO 16 (UART2 RX) |
| ESP32 TX → GPS RX | GPIO 17 (UART2 TX) |
| Limit switch signal | GPIO 27 (COM and NC contacts used) |
| HX711 DT | GPIO 32 |
| HX711 SCK | GPIO 33 |
| Red LED control | GPIO 25 |

Other firmware configuration:
- Limit switch contacts used: COM and NC
- LCD address: `0x27`
- LCD geometry: 16 columns × 2 rows
- Limit-switch pin mode: `INPUT_PULLUP`
- Load-cell calibration constant: `126.4576`

These are the final firmware pin assignments used in the prototype. The prototype has since been dismantled, so this table is not a new physical wiring verification.

The limit switch used COM and NC contacts. The firmware interprets a HIGH reading on GPIO 27 as `doorClosed = true`. The exact terminal-to-wire arrangement is not retained, so verify this logic level if rebuilding.

## Functional operation

The firmware reads temperature, humidity, illuminance, door state, load-cell weight and GPS information. It updates the LCD, prepares telemetry payloads, sends readings to the backend and processes backend responses.

The final integrated firmware includes the DHT22, BH1750, limit switch, HX711/load cell and GPS sensor identities. The MPU-6500 was removed from the final integrated version.

## Load-cell calibration limitation

The load cell used was rated at 20 kg. The final firmware contains a calibration constant of `126.4576`, and an earlier development sketch describes deriving a factor using a 177 g phone.

The calibration/zeroing issue remained unresolved in the final prototype. Therefore:
- The constant is a recorded firmware value, not a verified calibration.
- Weight acquisition was integrated, but accurate weight measurement was not established.
- Do not claim validated weight accuracy, repeatability, or correct zeroing.
- If the system is rebuilt, mechanically mount the load cell correctly, tare with no load, calibrate with known masses across the intended range, and record the observed error before using the weight data operationally.

## GPS note

The final firmware uses UART2 with GPIO 16 and GPIO 17. GPS reception depends on antenna placement and satellite visibility. The project archive does not contain a measured time-to-first-fix or positioning-accuracy report.

## Diagram

See [Truck Node pin map](../diagrams/TRUCK_NODE_PIN_MAP.mmd). It represents signal connections derived from the final firmware and does not include unverified power, ground, pull-up or load-cell bridge wiring.
