# 16-bit CPU Core Design (SystemVerilog)

This repository contains design files for a 16-bit CPU core developed as part of a Digital Design university course project. It includes the SystemVerilog modules that form the core microarchitecture.

---

### Modules

- **`pc.sv`** – **Program Counter:** Manages the address of the next instruction.
- **`ir.sv`** – **Instruction Register:** Holds the currently executing instruction.
- **`cu.sv`** – **Control Unit:** Implements the control finite state machine (FSM) and instruction decoder.
- **`rb.sv`** – **Register Bank:** 16 × 16-bit general-purpose registers.
- **`fu.sv`** – **Function Unit:** Performs arithmetic and logic operations.
- **`muxm.sv`** – **Multiplexer:** Selects the memory address source (Program Counter vs. bus).
- **`muxd.sv`** – **Multiplexer:** Selects the data source for write-back.
- **`muxb.sv`** – **Multiplexer:** Selects the source for the B-bus (`bbus`).
- **`mycpu.sv`** – **Top-level Module:** Connects all sub-modules into a unified CPU architecture.

---

### Note
This repository contains only the custom RTL design files. Course framework files and testbenches are omitted.
