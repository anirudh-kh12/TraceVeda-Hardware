# Hardware Components

This document describes the modules used in the final firmware and the role of each component. Exact breakout-board revisions and all electrical specifications were not retained in the project archive.

| Component | Node | Purpose / interface |
|---|---|---|
| ESP32 development board | Both | Main microcontroller; Wi-Fi, GPIO, I²C and UART |
| DHT22 | Both | Temperature and relative-humidity measurement; digital data |
| BH1750 | Both | Ambient illuminance measurement; I²C |
| I²C character LCD | Storage 20×4; Truck 16×2 | Local display |
| Limit switch | Both | Door/compartment state; digital input |
| Red LED | Both | Backend-directed visual alert output |
| NEO-6M GPS | Truck | Position data over UART |
| 20 kg load cell | Truck | Converts applied load into a small bridge signal |
| HX711 module | Truck | Amplifies and digitizes the load-cell signal |

## ESP32 development board

The ESP32 development modules acted as the controller for each node. They read sensors, update the local LCD, communicate with the backend over Wi-Fi and process backend responses. The boards were powered through USB during the prototype work.

## DHT22

The DHT22 measures temperature and relative humidity. In the final firmware its data pin is GPIO 4 on both nodes.

## BH1750

The BH1750 measures ambient illuminance in lux and communicates over I²C. The final firmware uses GPIO 21 for SDA and GPIO 22 for SCL.

## Character LCD

The Storage Node uses a 20×4 I²C LCD. The Truck Node uses a 16×2 I²C LCD. The final firmware declares I²C address `0x27` for both. The user confirmed that the LCD modules used 5 V in the prototype; the exact backpack pull-up arrangement is not recorded.

## Limit switch

The limit switch provides a digital input representing door/compartment state. Both final sketches configure GPIO 27 with `INPUT_PULLUP`. The exact NO/NC terminal selection is not currently known.

## NEO-6M GPS

The Truck Node uses a NEO-6M GPS receiver. GPS serial communication is configured on ESP32 UART2 using GPIO 16 as RX and GPIO 17 as TX.

## Load cell and HX711

The Truck Node uses a 20 kg load cell with an HX711 amplifier. The final firmware connects HX711 DT to GPIO 32 and SCK to GPIO 33. The calibration constant in the final firmware is `126.4576`; its final accuracy has not been independently confirmed.

## MPU-6500 experiment

An MPU-6500 was considered during an earlier Truck Node development stage. It was removed from the final integrated Truck Node firmware. The experiment sketch is retained in `firmware/Development/Truck_Node/` for historical context and is not part of the final hardware architecture.

## Voltage note

The prototype used a mixture of 3.3 V and 5 V module supplies; the LCD is an example of a 5 V module. The exact supply voltage for every sensor breakout and the I²C pull-up voltage are not recorded. ESP32 GPIOs are 3.3 V logic and are not 5 V tolerant. Do not recreate wiring without checking the exact module datasheets and logic-level compatibility.
