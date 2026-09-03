# Hardware and wiring notes

## Bill of materials

| Component | Quantity |
|---|---:|
| Arduino Mega 2560 with 12 V adapter | 1 |
| HSM-20G temperature/humidity sensor | 1 |
| Analog smoke sensor | 1 |
| Two-channel 5 V relay module | 1 |
| CPU cooling fan | 1 |
| 60 W lamp, holder, plug, and cable | 1 set |
| Servo motor | 1 |
| 10 kOhm resistor | 1 |
| 100 kOhm resistor | 1 |
| 47 uF capacitor | 1 |
| 10 kOhm potentiometer and LED | 1 each |
| Breadboard and jumper wires | 1 set |
| Acrylic panels and plywood base | As required |

The reported prototype cost was **BDT 4,810** in 2022.

## Sensor conditioning

The four-pin HSM-20G provides temperature output, humidity output, 5 V, and ground. In the documented circuit, the temperature path uses a 10 kOhm resistor. The humidity path uses a 100 kOhm resistor and 47 uF capacitor.

![HSM-20G conditioning circuit](../assets/sensor-circuit.png)

## Actuators

- **Lamp:** connected through one normally-open relay channel and used as the heat source.
- **Fan:** connected through the second normally-open relay channel and configured as an intake fan to displace heated air through the chamber outlet.
- **Servo:** powered from 5 V and ground, with its signal connected to a PWM-capable Arduino channel. It lifts a lightweight cardboard smoke-exhaust lid.

## Safety and validation

The report does not provide a complete pin-assignment table. Confirm channel selections in the LabVIEW front panel against the physical wiring. Test the smoke sensor, servo, fan, and lamp individually before closing the control loop.

The lamp circuit involves mains voltage. Reproduce it only with suitable protective enclosures, relay ratings, fusing, insulation, and qualified supervision.
