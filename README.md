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
## Simulation example
<img width="1236" height="570" alt="week8_bubble_wave" src="https://github.com/user-attachments/assets/fd83cc2f-11c6-4600-8a99-0712919bd042" />

The program reads four unsorted digits from the I/O input buffer, sorts them in descending order inside the register bank, and writes the sorted sequence back to the I/O port

* **Fetch–Execute Cycle (`st_r`):** The Control Unit alternates between Instruction Fetch (`INF`, reading instructions from memory addresses `34–38` on `a_out`) and Execute (`EX0`)
* **I/O Bus Control (`IOW`):** During cycles `66, 68, 70, 72`, the core decodes `IOW` instructions (`0x4201`–`0x4204`), asserts `iom_out = 1` and active-low `wen_out = 0`, and outputs the sorted values (**`9`**, **`8`**, **`7`**, **`1`**) onto `d_out[15:0]`
* **Program Halt (`HAL`):** At cycle `74`, the processor decodes the `HAL` instruction (`0xfe00`) and transitions permanently into the `HLT` state at cycle `75`

### Note
This repository contains only the custom RTL design files. Course framework files and testbenches are not presented.
