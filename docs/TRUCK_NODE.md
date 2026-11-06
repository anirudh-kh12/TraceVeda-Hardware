# Truck Node

## Overview

The Truck Node is the mobile ESP32 monitoring unit intended for the transport stage. Its final backend-integrated sketch is located at:

`firmware/Final_Integration/Truck_Node/Truck_Node_Backend.ino`

The user has confirmed that the physical Truck Node was implemented and integrated, and that its final LCD is 16×2. The supplied final sketch includes DHT22, BH1750, GPS, a limit switch, HX711/load cell, LCD, and red LED.

## Firmware-derived pin map

| Device / signal | ESP32 pin or bus | Notes |
|---|---|---|
| DHT22 data | GPIO 4 | `DHT_PIN` |
| BH1750 SDA | GPIO 21 | Shared I²C bus |
| BH1750 SCL | GPIO 22 | Shared I²C bus |
| LCD SDA | GPIO 21 | Shared I²C bus |
| LCD SCL | GPIO 22 | Shared I²C bus |
| GPS module TX → ESP32 RX | GPIO 16 | `GPS_RX`, UART2 |
| GPS module RX ← ESP32 TX | GPIO 17 | `GPS_TX`, UART2 |
| Limit switch | GPIO 27 | Configured `INPUT_PULLUP` |
| HX711 DT | GPIO 32 | `HX711_DT` |
| HX711 SCK | GPIO 33 | `HX711_SCK` |
| Red LED | GPIO 25 | Digital output |
| LCD address | `0x27` | Verify actual backpack address |
| LCD geometry | 16×2 | From final Truck Node sketch |

## Telemetry represented in firmware

The final integrated sketch maintains sensor identities for:
- `IOT-DHT22-001`
- `IOT-BH1750-001`
- `IOT-LIMIT-001`
- `IOT-HX711-001`
- `IOT-GPS-001`

The GPS payload includes validity information and coordinates when a valid location is available. The HX711 path reads weight and tracks changes relative to a previous reading.

## Timing values found in the final sketch

| Operation | Configured interval |
|---|---:|
| DHT22 sampling | 2,000 ms |
| BH1750 sampling | 500 ms |
| Weight sampling | 1,000 ms |
| Complete telemetry set | 5,000 ms |
| LCD refresh | 250 ms |
| LCD screen rotation | 2,000 ms |
| Serial diagnostic output | 1,000 ms |
| Wi-Fi check | 5,000 ms |

These are firmware scheduling intervals, not guaranteed sensor response times or measured end-to-end latency.

## HX711 calibration

The final sketch contains `HX711_CALIBRATION_FACTOR = 126.4576`. This value should be treated as a prototype calibration constant only. To reproduce or improve calibration:

1. Confirm the load-cell capacity and wiring.
2. Ensure the load cell is mounted mechanically without binding.
3. Tare with no load applied.
4. Apply one or more known masses across the intended range.
5. Adjust the calibration factor according to the HX711 library's sign and scale convention.
6. Record known mass, displayed mass, error, and calibration date.
7. Repeat after mechanical changes.

## GPS validation

Test outdoors with the antenna positioned for a clear sky view. Record whether a fix is valid, satellite count if available, and the time to first fix. An indoor no-fix state is not by itself proof of a faulty GPS module.

## MPU-6500 note

The older `Truck_Node_Test` sketch contains MPU-6500-related routines. The final backend-integrated Truck Node sketch explicitly states that MPU-6500 was removed. Therefore, MPU-6500 is classified as an earlier experiment, not a final integrated sensor in this repository's architecture.
