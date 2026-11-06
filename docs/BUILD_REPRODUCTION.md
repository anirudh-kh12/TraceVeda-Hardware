# Firmware Build and Reproduction Guide

## Recommended build order

1. Identify the exact ESP32 board/module used by the project.
2. Install the matching ESP32 Arduino board package.
3. Install required libraries.
4. Compile component test sketches individually.
5. Confirm sensor and display behaviour.
6. Configure local credentials and endpoint values.
7. Compile the Storage Node final integration sketch.
8. Compile the Truck Node final integration sketch.
9. Test backend connectivity and offline queue behaviour in a controlled environment.

## Source-level library dependencies

The integrated sketches include these libraries:

| Library/header | Use |
|---|---|
| `Wire.h` | I²C bus |
| `LiquidCrystal_I2C.h` | Character LCD |
| `BH1750.h` | Ambient-light sensor |
| `DHT.h` | DHT22 |
| `WiFi.h` | ESP32 Wi-Fi |
| `HTTPClient.h` | HTTP requests |
| `LittleFS.h` | Flash filesystem |
| `TinyGPSPlus.h` | GPS parsing (Truck Node) |
| `HX711.h` | Load-cell ADC (Truck Node) |

The source archive does not pin exact library versions or the ESP32 board package version. Do not invent version numbers. Record the versions that compile successfully in your own environment before claiming a reproducible build.

## Local configuration

The curated copies contain placeholders:
- `WIFI_SSID`
- `WIFI_PASSWORD`
- `BACKEND_BASE_URL`
- `DEVICE_API_KEY`

Replace these locally with values authorized by the project maintainers. Do not commit real credentials. If you move configuration to a header, add that filename to `.gitignore`.

## Destructive utility warning

`firmware/Little_FS_Clear/Little_FS_Clear.ino` is a filesystem-clearing utility. It can erase files stored in LittleFS. Do not flash or run it on a device containing queued telemetry or data you need to preserve.

## Reproduction limitations

Because the physical prototype is no longer assembled, a source compile does not prove that the complete circuit is electrically correct. A full hardware reproduction requires fresh physical wiring verification, module datasheets, safe power design, and sensor calibration.
