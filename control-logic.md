# Control logic

## Signal flow

1. LINX opens the connection to the Arduino Mega.
2. LabVIEW reads the configured temperature/humidity and smoke analog channels.
3. A formula node converts sensor voltages into temperature and humidity values.
4. Temperature is compared with the target and configured upper/lower band.
5. Digital outputs command the lamp and fan relay channels.
6. Smoke voltage is scaled to a servo pulse width and a displayed angle.
7. Measurements can be passed to the data-acquisition path for logging.
8. The VI closes the servo channel and LINX connection on shutdown.

![LabVIEW block diagram](../assets/labview-block-diagram.png)

## Temperature conversion

For measured voltage `V`, the report calculates thermistor resistance in kOhms:

```text
R = (5.0 - V) * 10.0 / V
```

It then applies the fitted conversion:

```text
Temperature = 281.583 * (1.0230^(1.0 / R)) * (R^-0.1227) - 150.6614
```

Humidity is calculated from humidity-channel voltage `H`:

```text
Humidity = (31 * H) - 12
```

## Servo mapping

Smoke voltage is linearly mapped from the calibrated interval `[VS_min, VS_max]` to a servo pulse range of 1350-2500 microseconds and a display range of 0-180 degrees. Values should be clamped to the calibrated interval before reuse in another build.

## Practical limitations

- Sensor calibration constants are specific to the original build and should be revalidated.
- The report describes PID control, while the visible block diagram also shows upper/lower-band comparisons. Review the VI in LabVIEW to confirm the exact deployed behavior.
- The report does not record final Arduino pin assignments.
- The prototype encountered faulty sensor, relay, and servo behavior during development, so startup diagnostics are recommended.
