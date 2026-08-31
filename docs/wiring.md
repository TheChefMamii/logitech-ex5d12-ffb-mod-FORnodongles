# Wiring (Confirmed)

All connectors have been identified, tested, and wired to the Pico.
This reflects the actual final wiring, verified with test scripts.

## Pin Summary

| Signal | Pico Pin |
|---|---|
| Button matrix (10 wires) | GP1–GP10 (see [button-mapping.md](button-mapping.md)) |
| Shift paddles — Left (Blue) | GP27 (ADC1) |
| Shift paddles — Right (Yellow) | GP28 (ADC2) |
| Shift paddles — GND (Yellow+Blue tied) | Pico GND |
| Shift paddles — VCC (Double Red) | Pico 3V3 (OUT) |
| Steering potentiometer — signal | GP26 (ADC0) |
| Steering potentiometer — VCC | Pico 3V3 (OUT) |
| Steering potentiometer — GND | Pico GND |
| Motor driver RPWM | GP21 |
| Motor driver LPWM | GP20 |
| Motor driver R_EN + L_EN (bridged together) | Pico 3V3 (OUT) |
| Motor driver VCC (logic) | Pico VBUS (5V) |
| Motor driver GND | Pico GND (common ground) |
| Motor driver B+/B- | External 19V power supply |
| Motor driver M+/M- | FFB motor |

## Important Notes

- **Common ground is critical.** The Pico's GND, the motor driver's
  GND, and the external power supply's GND must all be tied together,
  or PWM signals and ADC readings become unreliable.
- **R_EN and L_EN are bridged** into a single wire and held permanently
  high (3.3V) — direction/power is controlled entirely through
  RPWM/LPWM instead of toggling enables.
- **Motor direction is flipped from the pin names**, due to how the
  motor leads ended up wired: **RPWM (GP21) physically turns the wheel
  LEFT**, and **LPWM (GP20) physically turns it RIGHT**. This is handled
  in software by mapping the force sign accordingly — no need to
  re-wire anything.

## Verification

All of the above was confirmed using a combined test script that reads
the button matrix, both paddle channels, the potentiometer, and briefly
pulses the motor in both directions at reduced power (25% duty) to
confirm direction without stressing the hardware.
