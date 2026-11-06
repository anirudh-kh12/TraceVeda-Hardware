# Component Reference

This reference describes the components present in the supplied firmware. Exact module variants and board-level electrical details must be checked against the physical parts used in the prototype.

| Component | Node(s) | Firmware interface | Role |
|---|---|---|---|
| ESP32 development board | Both | GPIO, I²C, UART, Wi-Fi | Main controller; reads sensors and communicates with backend |
| DHT22 | Both | Single-wire digital data | Measures temperature and relative humidity |
| BH1750 | Both | I²C | Measures ambient illuminance in lux |
| I²C character LCD | Storage 20×4; Truck 16×2 | I²C | Local display of readings and device status |
| Limit switch | Both | Digital input with `INPUT_PULLUP` | Detects door/compartment switch state |
| Red LED | Both | Digital output | Indicates backend-directed alert state |
| GPS receiver | Truck | UART2 / serial | Provides latitude, longitude and fix-related information |
| Load cell | Truck | Through HX711 | Converts mechanical load into a small electrical signal |
| HX711 module | Truck | DT + SCK digital interface | Amplifies and digitizes the load-cell bridge signal |
| LittleFS | Both | ESP32 flash filesystem | Stores queued telemetry when network/backend upload is unavailable |

## Component notes

### DHT22

A capacitive humidity element and thermistor are used to report relative humidity and temperature. The supplied integrated firmware configures the data pin as GPIO 4 and declares `DHT22`. Allow the sensor's specified sampling interval; the Truck Node schedules DHT reads every 2 seconds.

### BH1750

A digital ambient-light sensor communicating over I²C. The integrated sketches initialize it in continuous high-resolution mode. The I²C bus uses GPIO 21 (SDA) and GPIO 22 (SCL).

### Character LCD with I²C backpack

The integrated Storage Node configures a 20-column, 4-row display. The integrated Truck Node configures a 16-column, 2-row display. Both use I²C address `0x27` in the supplied code. I²C address and voltage compatibility must be verified for the actual backpack.

### Limit switch

The sketches configure the input with `INPUT_PULLUP`. This means the expected wiring is typically a switch between the GPIO input and ground, but confirm the exact switch contacts and intended active state on the prototype before reproducing the circuit. The firmware variable represents a door-closed state; verify the mechanical mounting and contact selection.

### GPS receiver

The Truck Node uses ESP32 UART2 with RX GPIO 16 and TX GPIO 17. TinyGPSPlus parses the serial stream. A valid fix may not be available indoors; test outdoors with an appropriate antenna.

### Load cell and HX711

The HX711 connects to GPIO 32 (DT) and GPIO 33 (SCK). The firmware calibration factor is `126.4576`, with comments indicating the returned value is treated as grams before conversion to kilograms. Calibration is specific to the load cell, mechanical mounting, excitation voltage, and library convention. Validate using known reference masses and document the calibration method.

### Red LED

The red LED is configured on GPIO 25. The firmware comments indicate its state is updated from a backend response field named `red_led`. Use an appropriate current-limiting resistor and confirm LED polarity.

## Electrical safety and compatibility

- ESP32 GPIOs operate at 3.3 V logic and are not 5 V tolerant.
- Do not connect a 5 V logic output directly to an ESP32 GPIO.
- Check the pull-up voltage on I²C LCD backpacks; use level shifting where necessary.
- Confirm sensor supply voltage, logic levels, current requirements, and common-ground requirements from the exact module datasheets.
- Do not infer a module's safe voltage solely from the sensor IC name; breakout boards may include regulators or level shifters, or may omit them.
