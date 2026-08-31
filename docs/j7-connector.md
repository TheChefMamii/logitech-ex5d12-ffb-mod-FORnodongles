# J7 Connector — Shift Paddles (SOLVED)
## Final Result

The J7 socket has **4 physically separate wires** — do not confuse the
twisted GND/VCC pairs with the solid-color signal wires below, even
though they share color names:

| Wire (physical) | Pico Pin | Role |
|---|---|---|
| Twisted Yellow+Blue pair (separate wire, NOT the solid wires below) | Pico GND | Common ground |
| Twisted Red+Red pair (separate wire) | Pico 3V3 (OUT) | Shared supply |
| Solid Blue wire | GP27 (ADC1) | **Left paddle** signal |
| Solid Yellow wire | GP28 (ADC2) | **Right paddle** signal |

⚠️ **Do not tie the solid Yellow/Blue signal wires to GND** — the
GND connection comes from a separate twisted wire that only shares
color names with the signal wires, it is not the same physical
conductor.

For the full circuit context, see wiring.md

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
