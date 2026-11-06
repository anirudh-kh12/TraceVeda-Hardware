# TraceVeda Hardware Development Journal

This journal records the hardware development history using the available firmware archive and project context. It does not invent dates, measurements or test outcomes that were not retained.

## Project context

TraceVeda is a collaborative IoT-enabled monitoring and traceability project for herbal storage and transportation.

**Hardware contribution:** Anirudh Khatavkar  
**Original team repository:** https://github.com/vishnusharma131313-stack/TRACEVEDA

## Component exploration

The repository contains standalone sketches for ESP32, DHT22, BH1750, 16×4 and 20×4 LCDs, GPS, limit switch, and load cell/HX711 testing.

An MPU-6500 was considered during an earlier Truck Node development stage. It was removed from the final integrated design and is retained only as a development artifact.

## Storage Node

The final Storage Node firmware uses:
- ESP32 development board
- DHT22
- BH1750
- Limit switch
- 20×4 I²C LCD
- Red LED

The final sketch includes Wi-Fi/backend integration, telemetry transmission, backend response handling and offline queue-related logic.

## Truck Node

The final Truck Node firmware uses:
- ESP32 development board
- DHT22
- BH1750
- NEO-6M GPS
- Limit switch
- 20 kg load cell with HX711
- 16×2 I²C LCD
- Red LED

The final sketch includes sensor acquisition, GPS parsing, the integrated load-cell/HX711 reading path, backend telemetry transmission and backend response handling. The load-cell calibration/zeroing issue remained unresolved, so accurate weight measurement was not established.

## Final integration

Both nodes were fully integrated with the backend, including sending telemetry and receiving responses. The final firmware pin mappings are treated as the last mappings used in the implemented prototype.

The ESP32 development boards were powered through USB. The connected modules used a mixture of 3.3 V and 5 V supplies; the LCD is one example of a 5 V module.

## Prototype close-out

After development, the hardware was dismantled and the components borrowed from the IoT lab were returned. The original physical prototype is therefore unavailable for new inspection or photographs.

The pin maps and wiring documentation in this repository are reconstructed from the final firmware and the user's recollection. They are not represented as independently verified as-built schematics.

## Evidence and limitations

The archive retains source sketches, but not a complete dated test log, full photo record, exact module datasheets, final circuit schematics or measured performance report. Any future additions should use original records or new tests and should be clearly dated.
