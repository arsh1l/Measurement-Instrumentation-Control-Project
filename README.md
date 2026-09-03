# Temperature, Humidity, and Smoke Control System

![LabVIEW](https://img.shields.io/badge/LabVIEW-2019-FFDB00?logo=labview&logoColor=black)
![Arduino](https://img.shields.io/badge/Arduino-Mega_2560-00878F?logo=arduino&logoColor=white)
![Status](https://img.shields.io/badge/status-academic_prototype-blue)

An Arduino Mega and LabVIEW/LINX prototype that monitors temperature, humidity, and smoke, then responds with a heater lamp, cooling fan, and servo-operated ventilation lid. The project combines sensor conditioning, feedback control, actuator isolation, data acquisition, and a LabVIEW operator interface in one mechatronics system.

![Completed prototype](assets/prototype.jpg)

## What it does

- Reads temperature and humidity from an HSM-20G sensor.
- Detects smoke through an analog smoke sensor.
- Maintains a configurable temperature band by switching a 60 W lamp and CPU cooling fan through a two-channel 5 V relay.
- Maps the smoke-sensor voltage to a servo command that opens a lightweight exhaust lid.
- Displays live temperature, humidity, smoke level, actuator states, and servo angle in LabVIEW.
- Supports measurement logging from the front panel.

## System overview

| Layer | Components | Role |
|---|---|---|
| Sensing | HSM-20G, analog smoke sensor | Measures environmental conditions |
| Processing | LabVIEW VI, LINX, Arduino Mega 2560 | Converts signals and applies control logic |
| Actuation | 5 V relay, 60 W lamp, CPU fan, servo motor | Heats, cools, and vents the chamber |
| Interface | LabVIEW front panel | Configuration, monitoring, and data acquisition |

![LabVIEW front panel](assets/labview-front-panel.png)

## Control strategy

The VI converts analog readings into engineering values, compares temperature with user-defined upper and lower limits, and commands the heating or cooling relay. Smoke input is scaled between calibrated minimum and maximum values to generate the servo pulse width and displayed angle. The original report also describes PID-based feedback control.

Key conversion equations used in the report:

```text
R = (5.0 - V) * 10.0 / V
Temperature = 281.583 * (1.0230^(1.0 / R)) * (R^-0.1227) - 150.6614
Humidity = (31 * H) - 12

servo_us = 1350 + ((2500 - 1350) / (VS_max - VS_min)) * (input - VS_min)
servo_angle = 180 / (VS_max - VS_min) * (input - VS_min)
```

See [control logic](docs/control-logic.md) and [hardware notes](docs/hardware.md) for more detail.

## Repository structure

```text
assets/                    Project and interface images
docs/                      Design notes and original lab report
src/labview/               Original LabVIEW VI
README.md                  Project overview
```

## Requirements

- LabVIEW 2019 or a compatible version
- LINX toolkit / LabVIEW MakerHub support for Arduino
- Arduino Mega 2560
- The sensors and actuators listed in [docs/hardware.md](docs/hardware.md)

## Running the project

1. Build the low-voltage sensing circuit and actuator connections from the report and hardware notes.
2. Connect the Arduino Mega to the host computer and upload the LINX firmware required by your LabVIEW installation.
3. Open [`src/labview/temperature-smoke-servo-control.vi`](src/labview/temperature-smoke-servo-control.vi) in LabVIEW.
4. Select the serial port and configure the analog, relay, and servo channels for your wiring.
5. Set the target temperature, control band, and smoke calibration limits.
6. Run the VI and validate each actuator separately before operating the complete enclosure.

> [!CAUTION]
> The prototype switches a mains-powered lamp. Use a properly rated, enclosed relay module, strain relief, fusing, and supervision by a qualified person. Never handle exposed mains wiring while energized.

## Engineering highlights

- Integrated multiple analog sensors and three distinct actuator types.
- Isolated low-voltage control electronics from the lamp supply through relays.
- Designed and fabricated an acrylic environmental chamber with separate cooling and smoke-exhaust paths.
- Implemented sensor conversion, threshold control, servo scaling, visualization, and logging in LabVIEW.
- Diagnosed and replaced faulty sensors and relay hardware during prototyping.

## Team and context

Developed for **ME 4408** by Group IPE-06:

- Tahamid Arshil
- Jabir Ahmed
- Tarif Shahriar
- Syeda Tahiya Hassan
- Moho Hassan

Submitted in March 2022. The original report is included at [`docs/final-report.pdf`](docs/final-report.pdf).

## Notes

This repository preserves an academic prototype. Pin assignments and calibration limits should be verified against the physical build before reuse. No license has been assigned; all rights remain with the project authors.
