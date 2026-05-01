# RP2040 Bus Controller

This project uses a Raspberry Pi Pico / RP2040 as a bus controller for an 8-bit style CPU project.

The Pico basically acts as the memory / IO side of the CPU. It reads an 8-bit input bus, stores values into a 256 byte memory buffer, and can write values back out on an 8-bit output bus. Most of the core logic is written in ARM assembly because the point was to get closer to how the hardware is actually being controlled.

This is not meant to be some perfect production library. It is a hardware project / learning project that connects GPIO pins, memory logic, assembly code, and an OLED display together.

## What it does

- Reads an 8-bit input bus from GPIO pins
- Stores values in a 256 byte memory buffer
- Uses a pointer to select the current memory location
- Can move forward and backward by pages
- Can output the selected memory value back onto GPIO pins
- Has control pins for read, write, next, back, preload, input, and output
- Uses an OLED display to show the current state
- Uses ARM assembly for the main memory and runtime logic

## Hardware

This was built around the Raspberry Pi Pico / RP2040.

Current pin setup:

Input bus:
GP0 - GP7

Control pins:
GP8  = Read
GP9  = Write
GP10 = Next
GP11 = Back

Output bus:
GP12 - GP19

Extra control / PIO style pins:
GP20 = In
GP21 = Out
GP22 = Preload