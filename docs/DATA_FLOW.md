# Telemetry Data Flow

## Online path

```mermaid
sequenceDiagram
    participant Sensor as Sensors / peripherals
    participant ESP as ESP32 firmware
    participant Queue as RAM FIFO
    participant API as TraceVeda API
    participant Store as Backend storage

    Sensor->>ESP: Read measurement / state
    ESP->>ESP: Validate and construct JSON payload
    ESP->>Queue: Enqueue telemetry
    Queue->>API: POST /api/iot/readings
    API->>Store: Validate and persist
    Store-->>API: Result
    API-->>ESP: Response (including red_led state where applicable)
    ESP->>ESP: Update GPIO 25 LED state
```

## Offline/retry path

```mermaid
flowchart TD
    A["Sensor reading created"] --> B["RAM FIFO queue"]
    B --> C{"Backend upload succeeds?"}
    C -->|Yes| D["Remove acknowledged item"]
    C -->|No| E["Persist backlog to LittleFS"]
    E --> F{"Connectivity/backend available?"}
    F -->|No| E
    F -->|Yes| G["Read stored backlog"]
    G --> H["Retry upload"]
    H --> C
```

The exact retry/backoff and queue-capacity behaviour is defined in the firmware implementation. Do not infer guaranteed delivery under every power-loss or flash-failure scenario; test those cases explicitly before making a reliability claim.

## Sensor categories

| Category | Storage Node | Truck Node |
|---|---:|---:|
| Temperature | Yes | Yes |
| Relative humidity | Yes | Yes |
| Illuminance | Yes | Yes |
| Door state | Yes | Yes |
| GPS position | No | Yes |
| Payload weight | No | Yes |

## Backend interface

Both integrated sketches reference:

`POST /api/iot/readings`

The API base URL and device key are configuration values and are intentionally not included in this public documentation. Use the backend team's current API contract as the source of truth for payload fields and authentication.
