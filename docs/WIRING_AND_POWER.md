# Wiring and Power

## Source of the pin mappings

The GPIO assignments in this document are taken from the final Storage Node and Truck Node firmware. The user confirms that these were the last mappings used in the implemented prototype. The prototype was later dismantled and borrowed IoT-lab components were returned.

Accordingly, these diagrams are **firmware-derived connection records**, not fresh physical inspections or complete as-built electrical schematics.

## Storage Node

| ESP32 pin | Signal | Peripheral |
|---|---|---|
| GPIO 4 | Digital data | DHT22 |
| GPIO 21 | SDA | BH1750 and 20×4 LCD |
| GPIO 22 | SCL | BH1750 and 20×4 LCD |
| GPIO 27 | Digital input | Limit switch |
| GPIO 25 | Digital output | Red LED |

## Truck Node

| ESP32 pin | Signal | Peripheral |
|---|---|---|
| GPIO 4 | Digital data | DHT22 |
| GPIO 21 | SDA | BH1750 and 16×2 LCD |
| GPIO 22 | SCL | BH1750 and 16×2 LCD |
| GPIO 16 | UART2 RX | GPS TX → ESP32 RX |
| GPIO 17 | UART2 TX | ESP32 TX → GPS RX |
| GPIO 27 | Digital input | Limit switch |
| GPIO 32 | Digital input | HX711 DT |
| GPIO 33 | Clock output | HX711 SCK |
| GPIO 25 | Digital output | Red LED |

The LCD address declared in both final sketches is `0x27`.

## Power arrangement known from the project

- ESP32 development boards were powered through USB.
- The connected modules used a mixture of 3.3 V and 5 V supplies.
- The LCD is confirmed as an example of a 5 V module.
- Exact supply voltage and logic-level details for every individual breakout board are not available.

## Electrical precautions

- ESP32 GPIO pins are 3.3 V logic and are not 5 V tolerant.
- Confirm I²C pull-up voltage on the LCD backpack. If SDA/SCL are pulled up to 5 V, use a suitable level-shifting arrangement.
- Verify the supply and logic requirements of the exact DHT22, BH1750, GPS and HX711 breakout modules.
- Verify GPS UART voltage compatibility before connecting TX/RX.
- Use a current-limiting resistor for the red LED.
- Confirm load-cell bridge wiring from the exact load-cell/HX711 documentation; wire colours are not universal.
- Confirm common-ground and power-current requirements for the complete node.

## Not currently known

- Exact ESP32 board revision.
- Exact 3.3 V/5 V rail allocation for each individual sensor module.
- LCD backpack pull-up voltage and level-shifter arrangement.
- Limit switch NO/NC contact selection.
- Exact LED resistor value.
- Load-cell wire-colour mapping and HX711 excitation arrangement.

Do not treat the pin maps as sufficient instructions to rebuild the circuit. A future rebuild must verify these details against the exact modules and datasheets.
