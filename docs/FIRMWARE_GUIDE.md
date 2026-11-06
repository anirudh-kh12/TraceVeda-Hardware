# Firmware Guide

## Final integrated sketches

| Node | Curated final sketch | Role |
|---|---|---|
| Storage | `firmware/Final_Integration/Storage_Node/Storage_Node_Backend.ino` | Storage monitoring and backend integration |
| Truck | `firmware/Final_Integration/Truck_Node/Truck_Node_Backend.ino` | Transport monitoring and backend integration |

The original copies remain under `firmware/Storage_Node/` and `firmware/Truck_Node/` for development-history context. When making changes, choose one canonical file and keep the curated copy synchronized; avoid editing two copies independently.

## Common firmware responsibilities

### Peripheral initialization

The sketches initialize I²C, LCD, environmental sensors, GPIO inputs/outputs, Wi-Fi, and LittleFS. Truck firmware additionally initializes UART2/GPS and HX711.

### Sensor acquisition

Sensor readings are stored in variables and sampled at configured intervals. Failed DHT reads are treated as invalid rather than as trustworthy environmental values. BH1750 provides illuminance. The Truck Node continuously drains the GPS serial buffer into TinyGPSPlus.

### Payload construction

The firmware creates JSON payloads using stable sensor IDs and project/batch identifiers. The Truck Node keeps separate payload identities for DHT22, BH1750, limit switch, HX711, and GPS.

### Queue and offline handling

The firmware comments and implementation describe a RAM FIFO queue with LittleFS persistence for readings that cannot be uploaded. When connectivity or backend access returns, stored readings are retried. The intended priority and exact retry behaviour should be reviewed against the source before changing queue logic.

### Backend communication

The sketches post to `/api/iot/readings`. The Truck Node comments describe the `X-Device-Key` header and matching backend configuration. The base URL and API key have been replaced with placeholders in this portfolio copy.

### Backend-directed alert

The firmware reads a `red_led` value from the backend response and applies it to GPIO 25. This is a backend-directed indicator; the hardware firmware itself should not be described as independently implementing the backend's threshold policy.

## Libraries referenced by source

- `Wire`
- `LiquidCrystal_I2C`
- `BH1750`
- `DHT`
- `WiFi`
- `HTTPClient`
- `LittleFS`
- `TinyGPSPlus` (Truck Node)
- `HardwareSerial` (Truck Node)
- `HX711` (Truck Node)

Exact library versions are not pinned in the supplied source archive. Record the versions used for a reproducible build once verified from the development environment.
