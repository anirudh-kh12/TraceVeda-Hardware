# TraceVeda — Hardware Development Journal

This journal records the hardware-development path for TraceVeda from the available project context and firmware archive. It is intentionally conservative: it records known milestones without inventing dates, measurements, or unverified test outcomes.

## Project context

TraceVeda is a collaborative IoT-enabled monitoring and traceability project for herbal storage and transportation. The hardware contribution focused on ESP32-based Storage and Truck monitoring nodes.

**Hardware responsibility:** Anirudh Khatavkar  
**Project:** Collaborative team project  
**Original team repository:** https://github.com/vishnusharma131313-stack/TRACEVEDA

The physical prototype was assembled and integrated during development. After the work, the prototype was dismantled and components borrowed from the college IoT lab were returned. Therefore, current documentation is reconstructed from the final firmware, development sketches, and retained project context; it is not a post-dismantling physical inspection.

## Development record

### 1. Component exploration and individual testing

The hardware repository contains standalone test sketches for:

- ESP32 board/serial test
- DHT22 temperature and humidity sensor
- BH1750 ambient-light sensor
- 16×4 and 20×4 I²C LCD modules
- Limit switch
- GPS receiver
- Load cell with HX711 amplifier
- MPU6050/MPU-6500-family IMU experiment

The development process included I²C scanning and checking LCD device detection, DHT22 readings, MPU6050 I²C detection, HX711 raw readings, load-cell calibration investigation, and GPS testing. Exact numeric test logs and complete pass/fail records were not present in the supplied archive, so no numerical accuracy or reliability claims are made here.

### 2. Storage Node development

The Storage Node was developed around an ESP32 and the following final firmware peripherals:

- DHT22
- BH1750
- Door limit switch
- 20×4 I²C LCD
- Red LED

The project context records that Storage Node Wi-Fi integration was successfully tested. The final firmware contains backend telemetry, a backend-directed red LED state, and offline queue handling using RAM and LittleFS.

### 3. Truck Node development

The Truck Node was developed around an ESP32 and the following final firmware peripherals:

- DHT22
- BH1750
- GPS receiver
- Door limit switch
- Load cell and HX711
- 16×2 I²C LCD
- Red LED

The final firmware includes GPS parsing, weight measurement, telemetry payload construction, backend communication, and offline queue handling. The user confirms that the Truck Node was implemented and integrated. The final LCD used was 16×2.

The Truck Node development sketches also contain earlier IMU-related experimentation. The final backend-integrated sketch explicitly indicates that the MPU-6500 was removed; it is therefore documented as an experiment rather than a final integrated Truck Node sensor.

### 4. Final firmware and pin mapping

The pin assignments in the final backend-integrated sketches were the last and final mappings used in the implemented prototype. These mappings are treated as the authoritative firmware record in this repository.

Because the hardware was later dismantled, the pin maps and connection diagrams are labelled **firmware-derived**. They should not be described as independently re-verified after assembly.

### 5. Integration and backend communication

Both final sketches contain:
- Wi-Fi connectivity
- HTTP telemetry posting to `/api/iot/readings`
- Device-key header configuration
- RAM queueing and LittleFS backlog handling
- Backend response handling for the `red_led` output

The published copies replace credentials and endpoint configuration with placeholders. A working deployment requires configuration values from the project maintainers.

### 6. Prototype close-out

After hardware development and integration, the prototype was dismantled and borrowed IoT-lab components were returned. There is no currently available assembled prototype for new photographs or physical continuity checks.

## Evidence limitations

The supplied ZIP includes firmware and test sketches but does not include a complete set of:
- Dated bench photographs
- Final physical wiring photographs
- Signed-off electrical schematics
- Structured test logs with measured accuracy
- Component datasheet bundle
- Exact library-version lockfile

These should be added only if original records become available. Do not recreate photographs or present reconstructed diagrams as photographs of the real build.

## Future maintenance

If the hardware is rebuilt:
1. Confirm exact ESP32 board and module variants.
2. Verify every signal and power connection against the pin-map documents.
3. Record supply voltage and logic-level measurements.
4. Recalibrate the load cell using traceable known masses.
5. Test GPS outdoors.
6. Capture dated photographs and serial logs.
7. Update this journal with the actual date, test setup, observations, and evidence links.
