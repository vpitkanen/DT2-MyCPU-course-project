# 16-bit CPU Core Design (SystemVerilog)

This repository contains design files for a 16-bit CPU core developed as part of a Digital Design university course project. It includes the SystemVerilog modules that form the core microarchitecture.

---
```text
d_in[15:0] ----+----> [ IR ] ---- ins ----+----> [ Control Unit (cu) ] ----> wen_out, iom_out
               |        |                 |               ^
               |        |                 v               | nz[1:0]
               |        |             [ MUXB ] <-- bdat --+
               |        |                 |               |
               |        |               bbus              |
               |        |                 v               |
io_in[15:0] ---+----> [ MUXD ] <- fbus - [ FU ]           |
                        |                  ^              |
                      dbus               abus             |
                        v                  |              |
                      [ RB ] --------------+--------------+----> d_out[15:0]
                     (16 x 16)             |  (bdat)
                        |                  |
                        +--- abus ---------+---> [ PC ] -- pca --> [ MUXM ] --> a_out[15:0]
                                                   ^                 ^
                                                   |                 |
                                              ins (from IR)        abus
```

## RTL Modules

| File | Module | Description |
| :--- | :--- | :--- |
| `mycpu.sv` | Top-Level | Structural top-level integrating the control unit and datapath components. |
| `cu.sv` | Control Unit | Implements the control FSM (`RST`, `INF`, `EX0`, `HLT`) and combinational instruction decoder. |
| `pc.sv` | Program Counter | Manages next instruction address (`hold`, `PC+1`, signed relative branch, or jump). |
| `ir.sv` | Instruction Register | Latches the 16-bit instruction word fetched from memory during `INF`. |
| `rb.sv` | Register Bank | 16 × 16-bit register array (`R0–R7` user registers, `HR0–HR7` hidden registers). |
| `fu.sv` | Function Unit | Combinational ALU, saturated signed multiplier, custom user logic, and `N`/`Z` flag generation. |
| `muxm.sv` | Address Mux | Selects external address bus (`a_out`) source between `PC` and `abus`. |
| `muxb.sv` | B-Bus Mux | Selects `bbus` operand between `rb` output B and zero-extended 3-bit immediate. |
| `muxd.sv` | Write-Back Mux | Parameterized N-to-1 mux selecting `rb` write data (`fbus`, `d_in`, or `io_in`). |

---

### Note
This repository contains only the custom RTL design files. Course framework files and testbenches are not presented.
