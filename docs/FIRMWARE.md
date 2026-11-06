# Firmware Overview

## Main sketches

| Node | Sketch |
|---|---|
| Storage Node | `firmware/Storage_Node/Storage_Node.ino` |
| Truck Node | `firmware/Truck_Node/Truck_Node.ino` |

## Common firmware responsibilities

Both final sketches:
- Initialize the ESP32 peripherals and sensors.
- Acquire environmental and handling readings.
- Update the local LCD.
- Connect to Wi-Fi.
- Construct telemetry payloads.
- Send readings to the TraceVeda backend.
- Process backend responses, including the `red_led` state.
- Include RAM queueing and LittleFS-related offline handling.

The backend endpoint declared in the firmware is:

`/api/iot/readings`

The actual backend base URL and device API key are configuration values. They are represented by placeholders in the repository and must be supplied locally by an authorized project maintainer.

## Storage Node-specific firmware

The Storage Node handles DHT22, BH1750, limit-switch input, 20×4 LCD and red LED control.

## Truck Node-specific firmware

The Truck Node additionally handles:
- GPS serial communication using ESP32 UART2 and TinyGPSPlus.
- Load-cell measurement using HX711.
- Weight-related telemetry.
- 16×2 LCD display updates.

## Development folder

`firmware/Development/` retains earlier sketches that were used during development and debugging. They are not the canonical final firmware. The MPU-6500 experiment is retained there because it was considered and then removed from the final Truck Node.

## Libraries referenced by the source

- `Wire`
- `LiquidCrystal_I2C`
- `BH1750`
- `DHT`
- `WiFi`
- `HTTPClient`
- `LittleFS`
- `time`
- `TinyGPSPlus` (Truck Node)
- `HardwareSerial` (Truck Node)
- `HX711` (Truck Node)

The original archive does not provide a complete dependency lockfile or exact library versions. Add verified versions only after a successful build in the intended development environment.

## Credentials

Wi-Fi SSID/password, backend URL and device API key are placeholders in the curated final sketches. Never commit real credentials or private tokens.
