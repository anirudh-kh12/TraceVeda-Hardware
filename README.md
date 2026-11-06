# TraceVeda — Hardware & IoT Firmware

Hardware-focused portfolio repository for **TraceVeda**, a collaborative IoT-enabled herbal supply-chain traceability project.

This repository isolates the ESP32 firmware, component-level test sketches, hardware integration notes, pin maps, and engineering documentation. It is maintained as an individual contribution portfolio; TraceVeda itself remains a team project.

> **Role represented here:** Hardware development and IoT integration.  
> **Attribution:** The complete platform was developed collaboratively. This repository does not claim sole ownership of TraceVeda.

## What the hardware does

TraceVeda uses two ESP32-based monitoring nodes:

- **Storage Node:** monitors temperature, relative humidity, light intensity, and storage-door state. It displays local status and communicates readings to the backend over Wi-Fi.
- **Truck Node:** monitors temperature, relative humidity, light intensity, transport-compartment door state, payload weight, and GPS position. It displays local status and communicates readings to the backend over Wi-Fi.

Both final integration sketches post telemetry to the TraceVeda IoT endpoint (`/api/iot/readings`). The firmware includes RAM queuing and LittleFS-backed offline storage/retry logic. The backend can return a red-LED state to the device.

## Repository map

```text
TraceVeda-Hardware/
├── README.md
├── HARDWARE_DEVELOPMENT_JOURNAL.md
├── docs/
│   ├── ARCHITECTURE.md
│   ├── COMPONENTS.md
│   ├── STORAGE_NODE.md
│   ├── TRUCK_NODE.md
│   ├── FIRMWARE_GUIDE.md
│   ├── DATA_FLOW.md
│   ├── TESTING_AND_VALIDATION.md
│   └── KNOWN_GAPS.md
├── diagrams/
│   ├── SYSTEM_ARCHITECTURE.mmd
│   ├── STORAGE_NODE_PIN_MAP.mmd
│   └── TRUCK_NODE_PIN_MAP.mmd
├── firmware/
│   ├── Final_Integration/       # Curated copies of the final backend-integrated sketches
│   ├── Storage_Node/            # Original development/test sketches
│   ├── Truck_Node/              # Original development/test sketches
│   ├── Component_Testing/       # Individual component test sketches
│   └── Little_FS_Clear/         # Destructive filesystem utility; use with care
└── CONTRIBUTING.md
```

The original development sketches are retained for traceability. For the integrated prototype, start with `firmware/Final_Integration/`.

## Hardware overview

| Node | Measurements / state | Main interfaces |
|---|---|---|
| Storage | Temperature, humidity, illuminance, door state | GPIO, I²C, Wi-Fi |
| Truck | Temperature, humidity, illuminance, door state, weight, GPS position | GPIO, I²C, UART2, HX711 digital interface, Wi-Fi |

Pin assignments are documented in [Storage Node](docs/STORAGE_NODE.md) and [Truck Node](docs/TRUCK_NODE.md). They are derived from the supplied firmware and should be checked against the physical prototype before wiring.

## Quick start

1. Install the Arduino IDE or another ESP32-compatible Arduino build environment.
2. Install an ESP32 board package compatible with the target board.
3. Install the libraries used by the selected sketch:
   - `DHT sensor library` (Adafruit)
   - `Adafruit Unified Sensor` (dependency for the DHT library)
   - `BH1750`
   - `LiquidCrystal_I2C` (the API used by the sketch must match the installed library)
   - `TinyGPSPlus` (Truck Node)
   - `HX711` (Truck Node)
4. Open the desired `.ino` sketch.
5. Configure Wi-Fi, backend URL, and device API key using local, uncommitted configuration. Do not commit credentials.
6. Confirm the board variant, I²C address, voltage levels, and physical wiring.
7. Compile and test each sensor independently before running the integrated firmware.

### Configuration warning

The published sketches have been sanitized: Wi-Fi values, backend URLs, and device keys are placeholders. Replace them locally with values supplied for your own test environment. Do not put production secrets in source control. Prefer a separate local configuration header excluded by `.gitignore` or a build-time secrets mechanism.

## Important engineering notes

- ESP32 GPIO pins are **not 5 V tolerant**. Check each breakout board's logic-level requirements.
- Many I²C LCD backpacks are powered at 5 V and may pull SDA/SCL up to 5 V. Verify the backpack and use suitable level shifting if required.
- The firmware uses I²C address `0x27` for the LCD in the integrated sketches. Confirm the address on the actual module.
- GPS needs a suitable antenna and an open-sky view for reliable position fix; indoor no-fix behaviour is expected.
- The HX711 calibration factor in the Truck Node sketch is `126.4576`. Treat it as a prototype-specific calibration value, not a universal constant. Recalibrate with known weights after any mechanical or wiring change.
- The final Truck Node backend-integrated firmware uses a 16×2 LCD. The Storage Node backend-integrated firmware uses a 20×4 LCD.
- The older Truck Node test sketch contains MPU-6500 test logic, but the final Truck Node backend-integrated sketch explicitly removes MPU-6500. It is not listed as a final integrated sensor.

## Documentation

- [System architecture](docs/ARCHITECTURE.md)
- [Component reference](docs/COMPONENTS.md)
- [Storage Node](docs/STORAGE_NODE.md)
- [Truck Node](docs/TRUCK_NODE.md)
- [Firmware walkthrough](docs/FIRMWARE_GUIDE.md)
- [Telemetry data flow](docs/DATA_FLOW.md)
- [Testing and validation](docs/TESTING_AND_VALIDATION.md)
- [Wiring and power notes](docs/WIRING_AND_POWER.md)
- [Build and reproduction guide](docs/BUILD_REPRODUCTION.md)
- [Hardware development journal](HARDWARE_DEVELOPMENT_JOURNAL.md)
- [Known gaps and verification checklist](docs/KNOWN_GAPS.md)

## Team and individual contribution

TraceVeda is a collaborative project. This repository focuses on the hardware and embedded-firmware work contributed by **Anirudh Khatavkar**. Other project areas—including backend/database, frontend, blockchain, and research—were handled collaboratively by the wider team.

Original team repository:  
https://github.com/vishnusharma131313-stack/TRACEVEDA

Please refer to the original repository and its contributors for the complete project history and team attribution. This hardware-focused repository is a curated contribution portfolio, not a claim of sole authorship of the full TraceVeda platform.

## Licence and reuse

The supplied repository archive did not contain a root `LICENSE` file. Before publishing this derivative hardware-focused repository, coordinate with the repository owner/team and agree on the licence and attribution terms. Do not add a new permissive licence unless the relevant contributors have authorized it. Until that is resolved, treat the code as all-rights-reserved and include an attribution notice.
