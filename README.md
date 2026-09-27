# Physical Design: 4-bit Synchronous Counter (RTL to GDSII)

A complete ASIC physical design implementation of a 4-bit synchronous counter,
taken from Verilog RTL through to a manufacturable GDSII layout using the
open-source LibreLane flow on the SkyWater SKY130 PDK.

## Overview
This project demonstrates the full digital ASIC backend flow: Synthesis,
Floorplanning, Placement, Clock Tree Synthesis (CTS), Routing, and Static
Timing Analysis (STA), along with physical verification (DRC, LVS, Antenna).

## What I Learned
This project gave hands-on exposure to the complete ASIC implementation
flow — from writing synthesizable RTL, through floorplanning and placement
trade-offs, clock tree synthesis, routing, and interpreting STA timing
reports to identify and resolve setup/hold violations.
## Tools Used
- **LibreLane** (successor to OpenLane) — RTL-to-GDSII flow orchestration
- **Yosys** — RTL synthesis
- **OpenROAD** — Floorplanning, Placement, CTS, Routing
- **OpenSTA** — Static Timing Analysis
- **Magic / Netgen** — DRC and LVS physical verification
- **KLayout** — Layout viewing
- **SkyWater SKY130 PDK** — Open-source 130nm process design kit

## Design
A simple 4-bit synchronous counter with active-high reset:
```verilog
module counter (
    input clk,
    input reset,
    output reg [3:0] count
);
    always @(posedge clk or posedge reset) begin
        if (reset)
            count <= 4'b0000;
        else
            count <= count + 1;
    end
endmodule
```

## Flow Configuration
- PDK: `sky130A`
- Standard cell library: `sky130_fd_sc_hd`
- Clock period: 10ns (100MHz) — baseline run
- Die area: 100µm × 100µm (fixed, absolute sizing)

## Results (Baseline Run)
| Check | Result |
|---|---|
| Setup Timing Violations | None |
| Hold Timing Violations | None |
| Max Slew Violations | None |
| Max Capacitance Violations | None |
| DRC | Passed ✅ |
| LVS | Passed ✅ |
| Antenna | Passed ✅ |

## Timing Stress Test
To demonstrate timing analysis and violation debugging, the clock period was
deliberately reduced to induce a setup violation, then resolved. See
`docs/timing_analysis.md` for details.

## Repository Structure 
