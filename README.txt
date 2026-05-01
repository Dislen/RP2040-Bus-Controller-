# RP2040 Bus Controller

This project uses a Raspberry Pi Pico / RP2040 as a bus controller for an 8-bit style CPU project.

The Pico basically acts as the memory / IO side of the CPU. It reads an 8-bit input bus, stores values into a 256 byte memory buffer, and can write values back out on an 8-bit output bus. Most of the core logic is written in ARM assembly because the point was to get closer to how the hardware is actually being controlled.

This is not meant to be some perfect production library. It is a hardware project / learning project that connects GPIO pins, memory logic, assembly code, and an OLED display together.

This is part of a larger group project. The control signals are driven by an FPGA implementing a primitive 8-bit CPU, with discrete logic on breadboards handling arithmetic operations. The Pico sits on the bus as the memory and IO side of that system — it does not run the CPU logic, it just responds to whatever the FPGA tells it to do.
The core runtime and memory logic is written in ARM Thumb assembly. This was partly a learning exercise — I only knew x86 going in — and partly because getting direct control over the SIO registers without HAL overhead felt like the right approach for a tight bus interface loop. A working C implementation also exists as a fallback. The assembly version is faster, though some of that gap is probably down to the C version not being written very well.
Some of the logic in RUNTIME_STEP and the pin verification functions is a little rough. This was a physical hardware project and real wiring issues meant some workarounds ended up in the code that would look different in a cleaner environment. The DUMB_LOOP delay is empirical — 255 iterations proved stable on this specific hardware and never caused problems, so it never got changed into something more principled.
The OLED driver was not written from scratch. The font table and I2C init sequence came from reference implementations. Everything built on top of it is original.
This project taught me more about why C exists than any amount of reading about it would have.

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