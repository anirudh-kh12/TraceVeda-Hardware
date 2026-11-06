# TraceVeda — Hardware & IoT Firmware

Hardware-focused documentation and ESP32 firmware for **TraceVeda**, a collaborative IoT-enabled monitoring project for herbal storage and transportation.

This repository presents the hardware and embedded-firmware work contributed to the wider team project. It is not a claim of sole authorship of the complete TraceVeda platform.

**Individual contribution:** Hardware development and IoT integration  
**Original team repository:** https://github.com/vishnusharma131313-stack/TRACEVEDA

## Project overview

TraceVeda uses two ESP32-based nodes:

- **Storage Node:** monitors temperature, relative humidity, illuminance and storage-door state.
- **Truck Node:** monitors temperature, relative humidity, illuminance, compartment-door state and GPS position, and includes an integrated load-cell/HX711 weight-reading path. The load-cell calibration/zeroing issue remained unresolved, so accurate weight measurement was not established.

Both nodes were integrated with the TraceVeda backend. The firmware sends telemetry to the backend and processes the backend response, including the `red_led` control value.

## Hardware at a glance

| Node | Hardware represented in final firmware |
|---|---|
| Storage Node | ESP32 development board, DHT22, BH1750, limit switch, 20×4 I²C LCD, red LED |
| Truck Node | ESP32 development board, DHT22, BH1750, limit switch, NEO-6M GPS, 20 kg load cell with HX711, 16×2 I²C LCD, red LED |

The ESP32 development boards were powered through USB during the prototype work. The connected modules used a mixture of 3.3 V and 5 V supplies (for example, the LCD used 5 V). The limit switches used COM and NC contacts. Exact per-module supply and logic-level wiring is not fully recorded; see [Wiring and power](docs/WIRING_AND_POWER.md).

## Repository structure

```text
TraceVeda-Hardware/
├── README.md
├── .gitignore
├── DEVELOPMENT_JOURNAL.md
├── firmware/
│   ├── Storage_Node/
│   ├── Truck_Node/
│   ├── Component_Testing/
│   └── Development/
├── docs/
│   ├── ARCHITECTURE.md
│   ├── COMPONENTS.md
│   ├── STORAGE_NODE.md
│   ├── TRUCK_NODE.md
│   ├── WIRING_AND_POWER.md
│   ├── FIRMWARE.md
│   └── TESTING.md
└── diagrams/
    ├── SYSTEM_ARCHITECTURE.mmd
    ├── STORAGE_NODE_PIN_MAP.mmd
    └── TRUCK_NODE_PIN_MAP.mmd
```

## Final firmware

- [Storage Node firmware](firmware/Storage_Node/Storage_Node.ino)
- [Truck Node firmware](firmware/Truck_Node/Truck_Node.ino)

The final pin assignments in these sketches are the last mappings used in the implemented prototype. The hardware was later dismantled and borrowed IoT-lab components were returned. Therefore, the pin maps and diagrams in this repository are documented from the final firmware, not from a new physical inspection.

## Component testing and development

- `firmware/Component_Testing/` contains standalone sketches for the components tested during development.
- `firmware/Development/` contains earlier node integration sketches and debugging utilities retained for development history.
- The MPU-6500 experiment is retained under `Development/Truck_Node/` because it was considered in an earlier stage but removed from the final Truck Node.

## Documentation

- [System architecture](docs/ARCHITECTURE.md)
- [Component reference](docs/COMPONENTS.md)
- [Storage Node](docs/STORAGE_NODE.md)
- [Truck Node](docs/TRUCK_NODE.md)
- [Wiring and power](docs/WIRING_AND_POWER.md)
- [Firmware overview](docs/FIRMWARE.md)
- [Testing and validation](docs/TESTING.md)
- [Development journal](DEVELOPMENT_JOURNAL.md)

## Important limitations

- Circuit diagrams are firmware-derived signal maps, not independently verified as-built electrical schematics.
- No numerical accuracy, reliability, latency or pass-rate claims are made without retained test evidence.
- The exact load-cell calibration accuracy is not confirmed in this documentation.
- The supplied archive did not include a root `LICENSE` file. Reuse and licensing should be agreed with the relevant project contributors before publishing or redistributing code.

## Team acknowledgement

TraceVeda was developed collaboratively. This repository focuses on the hardware and embedded-firmware contribution. Backend/database, frontend, blockchain and research work were handled by other members of the project team.

For the complete project and team context, see the [original TraceVeda repository](https://github.com/vishnusharma131313-stack/TRACEVEDA).
