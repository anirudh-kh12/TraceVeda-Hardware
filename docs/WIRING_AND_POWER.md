# Wiring, Power and Electrical Design Notes

## Scope

The connection maps in this repository are reconstructed from the final integrated firmware. The user confirms that these were the final pin assignments used in the implemented prototype. The hardware has since been dismantled, so these diagrams are a firmware-derived record, not a fresh physical verification.

The diagrams show signal relationships and GPIO assignments. They are **not complete manufacturing schematics**: connector pinouts, power rails, grounds, pull-up resistors, decoupling capacitors, wire gauges, and exact module variants were not fully preserved in the source archive.

## Storage Node signal map

| ESP32 | Connected signal | Peripheral |
|---|---|---|
| GPIO 4 | Digital data | DHT22 |
| GPIO 21 | SDA | BH1750 and 20×4 LCD |
| GPIO 22 | SCL | BH1750 and 20×4 LCD |
| GPIO 27 | Digital input | Door limit switch |
| GPIO 25 | Digital output | Red LED driver/indicator |

I²C address declared for the LCD in firmware: `0x27`.

## Truck Node signal map

| ESP32 | Connected signal | Peripheral |
|---|---|---|
| GPIO 4 | Digital data | DHT22 |
| GPIO 21 | SDA | BH1750 and 16×2 LCD |
| GPIO 22 | SCL | BH1750 and 16×2 LCD |
| GPIO 16 | UART2 RX | GPS TX → ESP32 RX |
| GPIO 17 | UART2 TX | ESP32 TX → GPS RX |
| GPIO 27 | Digital input | Door limit switch |
| GPIO 32 | Digital input | HX711 DT |
| GPIO 33 | Digital output/clock | HX711 SCK |
| GPIO 25 | Digital output | Red LED driver/indicator |

I²C address declared for the LCD in firmware: `0x27`.

## Reconstructed signal diagrams

See:
- [`../diagrams/STORAGE_NODE_PIN_MAP.mmd`](../diagrams/STORAGE_NODE_PIN_MAP.mmd)
- [`../diagrams/TRUCK_NODE_PIN_MAP.mmd`](../diagrams/TRUCK_NODE_PIN_MAP.mmd)

These diagrams intentionally omit unverified power and ground wiring. Do not wire hardware from the signal map alone.

## Electrical design checks for a future rebuild

### ESP32 logic levels

ESP32 GPIOs are 3.3 V logic and are not 5 V tolerant. Confirm the output level of every connected module before connecting it to an ESP32 input.

### I²C LCD backpack

Some LCD backpacks are powered from 5 V and pull SDA/SCL up to their supply voltage. Confirm the pull-up arrangement. If the bus is pulled to 5 V, use a suitable bidirectional I²C level shifter or another electrically safe arrangement. Do not assume the backpack is 3.3 V safe.

### Sensor modules

Breakout boards vary. Check the exact DHT22 and BH1750 module documentation for supply range, logic levels, and onboard regulation/level shifting. Do not infer breakout-board safety solely from the sensor IC.

### Limit switch

The firmware configures GPIO 27 using `INPUT_PULLUP`. The usual arrangement is a contact between GPIO and ground, but the final contact selection (COM/NO/NC) and active mechanical state must be confirmed from the original assembly notes before rebuilding.

### Red LED

Use a current-limiting resistor or a suitable driver circuit. Confirm LED polarity and current against the actual LED specification. Do not connect an LED directly to a GPIO without current limiting.

### GPS

Confirm the GPS module's supply and UART logic levels. UART signal direction is crossed: GPS TX to ESP32 RX (GPIO 16), and ESP32 TX (GPIO 17) to GPS RX. Use an appropriate level shifter if required by the selected module.

### Load cell and HX711

The four load-cell bridge wires must be connected to the HX711 board according to the exact load-cell and amplifier pin labels/datasheets. Wire colours are not standardized across all load cells. Confirm excitation voltage, rated capacity, mechanical mounting, and calibration before use.

### Power budget

Do not assume the ESP32 3.3 V regulator can power every peripheral. Estimate current draw for the ESP32 (especially Wi-Fi transmit peaks), LCD backlight, GPS, sensors, and any other modules. Use an appropriately rated supply and common ground where required. Verify voltage at the modules under load.

## Rebuild sign-off checklist

- [ ] Exact ESP32 board variant identified.
- [ ] Exact sensor and display breakout variants identified.
- [ ] I²C address scan completed.
- [ ] SDA/SCL pull-up voltage measured or verified from schematic.
- [ ] UART voltage compatibility verified.
- [ ] Load-cell bridge wiring verified from datasheet.
- [ ] Supply capacity checked under Wi-Fi transmission.
- [ ] Grounding and polarity checked before power-on.
- [ ] First power-on performed with current-limited supply where practical.
- [ ] Final as-built schematic and photographs archived.
