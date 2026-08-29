# Button Matrix Mapping

The wheel's 16 buttons (14 face/shoulder buttons + 4-way D-Pad + PS
button) are wired as a diode matrix across only 10 Pico pins, rather
than a simple row/column grid. Each button bridges a unique pair of
pins when pressed.

## Wire Colors → Pico Pins

| Wire Color | Pico Pin | Index |
|---|---|---|
| White      | GP1  | 0 |
| Grey       | GP2  | 1 |
| Purple     | GP3  | 2 |
| Blue       | GP4  | 3 |
| Green      | GP5  | 4 |
| Yellow     | GP6  | 5 |
| Orange     | GP7  | 6 |
| Red        | GP8  | 7 |
| Brown      | GP9  | 8 (unused so far) |
| Black      | GP10 | 9 (unused so far) |

Brown and Black are not part of the 16-button matrix — all 16 buttons
were fully mapped using only the first 8 pins. These two are suspected
to belong to the shift paddles (see the J7 connector notes below).

## Button → Pin Pair Mapping

| Button | Pin Pair (index) |
|---|---|
| X | 0, 7 |
| O | 1, 6 |
| Triangle | 0, 6 |
| Square | 1, 7 |
| L1 | 4, 5 |
| R1 | 0, 5 |
| L2 | 1, 5 |
| R2 | 2, 5 |
| L3 | 1, 3 |
| R3 | 0, 3 |
| Select | 4, 3 |
| Start | 2, 3 |
| PS | 1, 4 |
| D-Pad Left | 2, 6 |
| D-Pad Up | 4, 7 |
| D-Pad Right | 2, 7 |
| D-Pad Down | 4, 6 |

Found using a simple matrix-scan test script that prints which pin
pair gets pulled low when a button is pressed.

## Notes / Open Questions

- Brown (GP9) and Black (GP10) are still unused — most likely tied to
  the shift paddles connector (J7), not the main button matrix.
- No diodes were added externally; the matrix behaves correctly as-is,
  suggesting diodes already exist somewhere in the original wiring
  harness or button PCB.
