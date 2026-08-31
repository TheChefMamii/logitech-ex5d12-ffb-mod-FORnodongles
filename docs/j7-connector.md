# J7 Connector — Shift Paddles (SOLVED)

## Final Result

The J7 socket carries the two shift paddles as **two independent
analog (RC-filtered resistive) channels**, not a digital shift
register and not a shared resistor ladder. Each paddle only affects
its own wire; the other stays at its baseline.

| Wire | Pico Pin | Role |
|---|---|---|
| Yellow + Blue (tied together) | Pico GND | Common ground |
| Double Red | Pico 3V3 (OUT) | Shared supply |
| Blue | GP27 (ADC1) | **Left paddle** signal |
| Yellow | GP28 (ADC2) | **Right paddle** signal |

## Measured Values

| State | Blue (GP27) | Yellow (GP28) |
|---|---|---|
| Idle (nothing pressed) | ~400 | ~53500–59500 |
| Right paddle held | ~400 (unchanged) | ~11000–11200 |
| Left paddle held | ~50700–51100 | ~59300 (unchanged) |
| Both paddles held | ~47700–48000 | ~12700–12750 |

Values rise/fall gradually rather than jumping instantly — confirming
the C20/C27 capacitors on the original board act as an RC debounce
filter, not just noise suppression. Reading these pins requires
letting the value settle for a short moment rather than reacting
to the very first sample.

## Reading Logic

```python
LEFT_PADDLE_THRESHOLD = 20000   # left pressed if GP27 > this
RIGHT_PADDLE_THRESHOLD = 30000  # right pressed if GP28 < this

def read_paddles(left_raw, right_raw):
    left_pressed = left_raw > LEFT_PADDLE_THRESHOLD
    right_pressed = right_raw < RIGHT_PADDLE_THRESHOLD
    return left_pressed, right_pressed
```

## Status: Closed

No further hardware investigation needed for J7. This logic is folded
into the main firmware.
