# RP2040 Bus Controller

This project uses a Raspberry Pi Pico / RP2040 as the memory and IO side of an 8-bit CPU project.

The larger system is a group-built 8-bit CPU. Control signals are driven by an FPGA implementing the CPU control logic, while arithmetic operations are handled with discrete logic on breadboards. The Pico sits on the bus and responds to those control signals. It does not run the CPU logic itself.

The Pico reads an 8-bit input bus, stores values into a 256 byte memory buffer, and can write values back out on an 8-bit output bus. Most of the core runtime and memory logic is written in ARM Thumb assembly because I wanted closer control over how the hardware was being driven.

This is not a polished production library. It is a hardware project / learning project that connects GPIO pins, memory logic, assembly code, FPGA control signals, breadboarded arithmetic logic, and an OLED display together.

A working C implementation also exists as a fallback. The assembly version is faster in this project, although this should not be read as a general claim that hand-written assembly always beats C.

Some of the logic in `RUNTIME_STEP` and the pin verification functions is rough because this was tested on real physical wiring, not a clean simulator. A few workarounds exist because of hardware noise and breadboard behavior. The `DUMB_LOOP` delay is empirical: 255 iterations proved stable on this specific setup, so I left it alone.

The OLED driver was not written completely from scratch. The font table and I2C initialization sequence came from reference implementations. The project-specific display logic built on top of it is original.

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