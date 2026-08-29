# Logitech E-X5D12 → DIY Force Feedback USB Steering Wheel

This project documents the process of tearing down a Logitech E-X5D12
steering wheel (original dongle lost) and replacing its electronics
with a Raspberry Pi Pico (RP2040) and a BTS7960 motor driver, turning
it into a USB-connected wheel with force feedback support.

**Hardware**
<img width="3060" height="4080" alt="20260829_200107" src="https://github.com/user-attachments/assets/af4e6dc2-3523-4c37-905c-5cb93a9d1831" />
- A Logitech E-X5D12 wheel (or similar — the general process applies to
  most direct-drive-style wheels with a separate mainboard)
  
- Raspberry Pi Pico (RP2040), CircuitPython-compatible
- 
- BTS7960 motor driver module
- 
- A power supply for the motor (this project uses a repurposed 19V

  laptop adapter — check your motor's rated voltage/current first)
  
- Jumper wires / spare wire for extending connectors
  
- A multimeter (essential — you'll be using it constantly to identify
  unknown connectors)

**Tools**
- Screwdriver set(flat ones)

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

Once every socket is identified, follow the wiring diagram in
[`docs/wiring.md`](docs/wiring.md) to connect each one to the Pico and
the BTS7960 motor driver. This is also where you'll wire in the
external power supply for the motor.
