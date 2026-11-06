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
| Limit switch signal | GPIO 27 |
| HX711 DT | GPIO 32 |
| HX711 SCK | GPIO 33 |
| Red LED control | GPIO 25 |

Other firmware configuration:
- LCD address: `0x27`
- LCD geometry: 16 columns × 2 rows
- Limit-switch pin mode: `INPUT_PULLUP`
- Load-cell calibration constant: `126.4576`

These are the final firmware pin assignments used in the prototype. The prototype has since been dismantled, so this table is not a new physical wiring verification.

## Functional operation

The firmware reads temperature, humidity, illuminance, door state, load-cell weight and GPS information. It updates the LCD, prepares telemetry payloads, sends readings to the backend and processes backend responses.

The final integrated firmware includes the DHT22, BH1750, limit switch, HX711/load cell and GPS sensor identities. The MPU-6500 was removed from the final integrated version.

## Load-cell calibration note

The 20 kg value is the load-cell capacity reported by the user. The final firmware contains a calibration constant of `126.4576`. Earlier development code describes calibrating with a 177 g phone, but the final accuracy and zero stability of the assembled prototype have not been confirmed. Do not present the constant as a verified calibration result.

## GPS note

The final firmware uses UART2 with GPIO 16 and GPIO 17. GPS reception depends on antenna placement and satellite visibility. The project archive does not contain a measured time-to-first-fix or positioning-accuracy report.

## Diagram

See [Truck Node pin map](../diagrams/TRUCK_NODE_PIN_MAP.mmd). It represents signal connections derived from the final firmware and does not include unverified power, ground, pull-up or load-cell bridge wiring.
