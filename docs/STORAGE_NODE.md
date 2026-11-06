# Storage Node

## Overview

The Storage Node is the stationary ESP32-based monitoring unit intended for the storage environment. Its final backend-integrated sketch is located at:

`firmware/Final_Integration/Storage_Node/Storage_Node_Backend.ino`

The supplied final sketch configures DHT22, BH1750, a door limit switch, a 20×4 I²C LCD, and a red LED.

## Firmware-derived pin map

| Device / signal | ESP32 pin or bus | Notes |
|---|---|---|
| DHT22 data | GPIO 4 | `DHT_PIN` |
| BH1750 SDA | GPIO 21 | Shared I²C bus |
| BH1750 SCL | GPIO 22 | Shared I²C bus |
| LCD SDA | GPIO 21 | Shared I²C bus |
| LCD SCL | GPIO 22 | Shared I²C bus |
| Limit switch | GPIO 27 | Configured `INPUT_PULLUP` |
| Red LED | GPIO 25 | Digital output |
| LCD address | `0x27` | Verify actual backpack address |
| LCD geometry | 20×4 | From final Storage Node sketch |

## Functional sequence

1. Initialize serial diagnostics.
2. Initialize I²C and the LCD.
3. Initialize DHT22 and BH1750.
4. Configure the limit switch input and red LED output.
5. Initialize the flash filesystem and network-related services.
6. Read sensors on their scheduled intervals.
7. Build sensor-specific telemetry payloads.
8. Queue readings in RAM and persist backlog to LittleFS when upload is unavailable.
9. Upload queued readings to the TraceVeda API when connectivity is available.
10. Apply the backend-returned `red_led` state to GPIO 25.
11. Refresh the LCD with current readings and status.

## Wiring reference

See [`../diagrams/STORAGE_NODE_PIN_MAP.mmd`](../diagrams/STORAGE_NODE_PIN_MAP.mmd). It is a firmware-derived signal map, not a substitute for a verified electrical schematic. Power, ground, pull-up, resistor, and connector details must be filled in from the actual assembled circuit.
