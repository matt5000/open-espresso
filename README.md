# Open Espresso

Open-source espresso machine firmware for the Gaggia Classic (pre-2015).

Single ESP32-S3 MCU. PID temperature control. Pressure and flow profiling. Web interface. BLE scale support.

## Status

**Design phase.** The firmware is not yet written. The [design document](docs/DESIGN_DOCUMENT.md) specifies the full architecture, requirements, and interface contracts.

## What This Project Is

A ground-up firmware for retrofitting a Gaggia Classic with modern espresso features:

- **PID temperature control** — ±1°C steady-state accuracy
- **Pressure and flow profiling** — multi-phase shot profiles with configurable transitions
- **Web interface** — real-time shot monitoring, profile editor, config management
- **TFT display** — local temperature, pressure, flow, and shot timer
- **BLE scale support** — automatic weight-based shot stopping
- **OTA updates** — firmware updates over WiFi
- **Safety-first** — three-layer defense (software + watchdog + hardware thermal fuse)

## Inspiration

This project is inspired by [Gaggiuino](https://github.com/Zer0-bit/gaggiuino), an espresso machine modification project. Open Espresso is an independent, clean-room rewrite — no code from Gaggiuino is used.

## Documentation

- [Design Document (HTML)](docs/DESIGN_DOCUMENT.html) — multi-page browsable version with SVG diagrams
- [Design Document (Markdown)](docs/DESIGN_DOCUMENT.md) — canonical source, full architecture, requirements, ICDs, test plan
- [Design Document Guidelines](docs/DESIGN_DOCUMENT_GUIDELINES.md) — how to write design docs (IEEE 1016/29148)

## Hardware Target

- **MCU:** ESP32-S3 (WROVER recommended for PSRAM)
- **Display:** 2.8" TFT (ILI9341) with capacitive touch (CST816)
- **Sensors:** MAX31855 thermocouple, ADS1115 pressure transducer, hall-effect flow sensor
- **Actuators:** SSR (boiler), AC dimmer (pump), solenoid (3-way valve)
- **Machine:** Gaggia Classic, pre-2015 model (non-Pro)

See the [design document](docs/DESIGN_DOCUMENT.md) sections 13-14 for full BOM and machine-specific notes.

## License

[MIT](LICENSE)
