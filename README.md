# Logitech E-X5D12 → DIY Force Feedback USB Steering Wheel

This project documents the process of tearing down a Logitech E-X5D12
steering wheel (original dongle lost) and replacing its electronics
with a Raspberry Pi Pico (RP2040) and a BTS7960 motor driver, turning
it into a USB-connected wheel with force feedback support.

## What You'll Need

**Hardware**
- A Logitech E-X5D12 wheel (or similar)
- Raspberry Pi Pico (RP2040), CircuitPython-compatible
- BTS7960 motor driver module
- A power supply for the motor (this project reuses a 19V laptop adapter)
- wires
- A multimeter (essential)

**Tools**
- Screwdriver set (flathead)
- 
## Getting Started

## Step 1: Open the Wheel and Remove the Mainboard

Before touching any wiring, start by opening up the wheel housing:

1. Flip the wheel over and remove all the screws holding the back
   panel in place.
2. Carefully separate the back panel from the front housing — there
   are usually a few plastic clips in addition to the screws, so don't
   force it.
3. Locate the original mainboard and unplug every connector attached
   to it (motor, buttons, sensors, etc.) before lifting it out.
4. **Do not throw the original mainboard away.** Even though it won't
   be used going forward, it's extremely useful as a reference — it
   tells you which socket carries which signal, based on where it
   connects on the board and the trace layout around it.

   <img width="3060" height="4080" alt="20260829_195954" src="https://github.com/user-attachments/assets/a50517b1-5335-42bc-a789-30e3e54ff806" />

## Step 2: Pull Out the Sockets

Once the mainboard is out, you're left with a bunch of loose
connectors coming from the wheel's internals (motor, potentiometer,
button matrix, shifter paddles, etc.). For each one:

1. Leave the connector as-is, or solder a jumper wire onto each pin if
   the connector housing is hard to probe directly.
2. Don't cut or shorten any wires yet — you want enough slack to reach
   the new controller board comfortably.

To figure out what each socket actually is (steering sensor, motor,
buttons, etc.), check [`docs/parts-inventory.md`](docs/parts-inventory.md)
— every connector identified so far is listed there, along with which
ones are still unconfirmed.

## Step 3: Wire Everything to the New Controller
<img width="901" height="691" alt="SCHEMA OF MODDED MAINBOARD" src="https://github.com/user-attachments/assets/aae998e4-e595-4a98-881c-ba001f900201" />
Every connector was identified and wired using a hands-on approach —
testing each socket directly with the Pico rather than guessing from
datasheets alone:

- The button matrix (10 wires) was mapped by connecting all 10 wires
  to GPIO pins and running a scan script that prints which pin pair
  gets triggered per button press.
- The mystery 4-pin connector for the shift paddles turned out to be
  two independent RC-filtered analog channels rather than a digital
  shift register.
- The motor driver's direction pins were tested at reduced power
  (25% duty) before running at full power.

Full final pinout: [`docs/wiring.md`](docs/wiring.md)
Button mapping details: [`docs/button-mapping.md`](docs/button-mapping.md)
Shift paddle investigation: [`docs/j7-connector.md`](docs/j7-connector.md)
