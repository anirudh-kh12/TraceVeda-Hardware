# Storage Node

## Overview

The Storage Node is the stationary monitoring unit built around an ESP32 development board.

### Final hardware

- ESP32 development board
- DHT22
- BH1750
- Limit switch
- 20×4 I²C LCD
- Red LED

### Final firmware

`firmware/Storage_Node/Storage_Node.ino`

## Firmware-derived pin mapping

| Peripheral signal | ESP32 connection |
|---|---|
| DHT22 data | GPIO 4 |
| BH1750 SDA | GPIO 21 |
| BH1750 SCL | GPIO 22 |
| LCD SDA | GPIO 21 |
| LCD SCL | GPIO 22 |
| Limit switch signal | GPIO 27 (COM and NC contacts used) |
| Red LED control | GPIO 25 |

Other firmware configuration:
- Limit switch contacts used: COM and NC
- LCD address: `0x27`
- LCD geometry: 20 columns × 4 rows
- Limit-switch pin mode: `INPUT_PULLUP`

These are the final firmware pin assignments used in the prototype. The prototype has since been dismantled, so this table is not a new physical wiring verification.

The limit switch used COM and NC contacts. The firmware interprets a HIGH reading on GPIO 27 as `doorClosed = true`. The exact terminal-to-wire arrangement is not retained, so verify this logic level if rebuilding.

## Functional operation

The firmware initializes the display, sensors, GPIO, Wi-Fi and filesystem. It periodically reads the environmental sensors and door input, updates the LCD, constructs telemetry payloads and sends readings to the TraceVeda backend.

The code includes RAM queueing and LittleFS-backed handling for readings that cannot be sent immediately. The firmware also reads the backend response and updates the red LED using the returned `red_led` state.

## Diagram

See [Storage Node pin map](../diagrams/STORAGE_NODE_PIN_MAP.mmd). It represents signal connections derived from the final firmware and does not include unverified power, ground, pull-up or resistor details.
