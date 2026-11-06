# Testing and Validation

## Test assets present in the supplied repository

The following independent test sketches were present in the uploaded project archive and are retained under `firmware/Component_Testing/`:

| Test sketch | Component / purpose |
|---|---|
| `ESP32_Test` | Basic ESP32 serial/board test |
| `DHT22_Test` | Temperature and humidity sensor |
| `BH1750_Test` | Ambient light sensor |
| `LCD_16x4_Test` | 16×4 character LCD |
| `LCD_20x4_Test` | 20×4 character LCD |
| `Limit_Switch_Test` | Digital switch input |
| `MPU6050_Test` | Earlier IMU component experiment |
| `GPS_Test` | GPS serial parsing |
| `Load_Cell_HX711_Test` | Load cell and HX711 readings |

Other node-level sketches are retained under their respective node folders. They represent development/testing stages and should not be mistaken for the final integrated firmware.

## Evidence to add before public release

The source archive contains firmware but does not contain a complete hardware test report, verified circuit drawings, or photographs of every test setup. Add actual evidence rather than inventing results.

For each component, capture:

| Field | What to record |
|---|---|
| Test ID | Unique identifier, e.g. `T-DHT22-01` |
| Date | Actual test date |
| Hardware | Exact module/board and wiring |
| Firmware | Sketch filename and revision |
| Procedure | Steps performed |
| Expected result | Datasheet/functional expectation |
| Observed result | Actual serial/LCD output |
| Status | Pass / fail / needs investigation |
| Evidence | Photograph, serial log, or measurement |
| Notes | Errors, calibration, environmental conditions |

## Recommended validation checklist

### Component level

- [ ] ESP32 boots and serial output is stable.
- [ ] DHT22 returns plausible temperature and humidity values.
- [ ] BH1750 is detected and returns changing lux values under changing light.
- [ ] LCD initializes at the correct address and geometry.
- [ ] Limit switch transitions are correct for open/closed positions.
- [ ] GPS produces valid coordinates outdoors.
- [ ] HX711 detects the load cell, tares correctly, and responds to known masses.
- [ ] Red LED output and resistor/current arrangement are verified.

### Integration level

- [ ] All I²C devices coexist without address conflict.
- [ ] Sensor reads do not block critical firmware tasks unexpectedly.
- [ ] Both nodes connect to the intended Wi-Fi network.
- [ ] Telemetry is accepted by the backend with the expected device identity.
- [ ] Backend response controls the red LED as intended.
- [ ] Offline queue survives a network interruption.
- [ ] Backlog uploads after network recovery.
- [ ] Repeated power cycles do not corrupt or unexpectedly erase queued data.
- [ ] Truck GPS and weight values are sensible in the actual transport setup.

## Results policy

No numerical accuracy, latency, reliability, or pass-rate claims are made here because the ZIP did not contain a structured test-results report. Populate the results table only with observations you can substantiate from your lab notes, serial logs, or repeatable tests.
