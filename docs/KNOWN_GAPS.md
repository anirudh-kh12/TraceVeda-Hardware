# Known Gaps and Verification Checklist

This file separates facts visible in the supplied source from details that still need confirmation.

## Confirmed from the supplied final firmware

- Storage Node uses DHT22, BH1750, limit switch, 20×4 LCD, and red LED.
- Truck Node uses DHT22, BH1750, GPS, limit switch, HX711/load cell, 16×2 LCD, and red LED.
- Both integrated sketches use I²C SDA GPIO 21 and SCL GPIO 22.
- Both use DHT22 data GPIO 4, limit switch GPIO 27, and red LED GPIO 25.
- Truck GPS uses UART2 with RX GPIO 16 and TX GPIO 17.
- Truck HX711 uses DT GPIO 32 and SCK GPIO 33.
- The integrated sketches reference `/api/iot/readings`.
- Both include LittleFS-related offline queue logic.

## Must be verified against the physical prototype

- [ ] Exact ESP32 board model and revision.
- [ ] Exact DHT22 breakout and supply/logic-level arrangement.
- [ ] BH1750 module supply and logic-level arrangement.
- [ ] LCD backpack supply voltage, I²C pull-up voltage, and address.
- [ ] Exact limit-switch terminal selection (COM/NO/NC) and mechanical state.
- [ ] Red LED wiring and current-limiting resistor value.
- [ ] GPS module model, supply, antenna, and UART voltage levels.
- [ ] Load-cell capacity, wiring colours, mounting, and mechanical load path.
- [ ] HX711 board supply and output logic level.
- [ ] Power source and current budget for the complete node.
- [ ] Final pin map matches the actual assembled wiring.

## Source inconsistencies / development-stage differences

1. The repository README describes a 16×2 Truck Node LCD; the final Truck Node integrated sketch also declares 16×2. The separate component tests include both 16×4 and 20×4 displays. The user confirms 16×2 is the final Truck Node display.
2. The older `Truck_Node_Test` includes MPU-6500 code. The final `Truck_Node_Backend` explicitly says MPU-6500 was removed. Do not show MPU-6500 as part of the final integrated Truck Node.
3. The final Storage Node sketch declares a 20×4 LCD.
4. The component-testing folder includes an MPU6050 test sketch. That is a standalone experiment, not evidence of final integration.
5. No final GSM/4G modem implementation was identified in the supplied hardware `.ino` files. Do not claim GSM/4G connectivity as implemented.
6. The supplied archive contains no hardware-specific Markdown manual, verified schematic files, structured test-result report, or prototype photo set.

## Configuration and publication safety

- [ ] Ensure no real Wi-Fi SSID/password remains in any copied sketch.
- [ ] Ensure no production API key, backend URL, token, or personal data remains.
- [ ] Keep local secrets outside version control.
- [ ] Check with the team/repository owner before publishing a derivative code repository.
- [ ] Resolve the licence: the supplied ZIP did not include a root `LICENSE` file.


## Prototype availability

The prototype was dismantled after the development work and the borrowed IoT-lab components were returned. No new physical inspection or photographs can be taken from the original assembly. See [`../HARDWARE_DEVELOPMENT_JOURNAL.md`](../HARDWARE_DEVELOPMENT_JOURNAL.md) for the development record and [`WIRING_AND_POWER.md`](WIRING_AND_POWER.md) for reconstruction limitations.
