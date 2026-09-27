# Timing Analysis: Clock Period Sweep

To demonstrate Static Timing Analysis and violation resolution, the counter
design was run at progressively realistic clock periods.

## Run 1: CLOCK_PERIOD = 0.3ns (300ps)
**Result: FAILED.** Setup violations across nearly all PVT corners
(ff, ss, tt), hold violations across all corners. Flow aborted.

Example violating path (nom_tt_025C_1v80 corner, flip-flop `_20_` to
output `count[0]`):
- Data arrival time: 0.9527 ns
- Data required time: -0.0100 ns
- **Slack: -0.9627 ns (VIOLATED)**

The flip-flop clock-to-output delay alone (0.4677ns) plus output buffer
delay (0.1675ns) exceed the requested 0.3ns period — an unachievable
constraint for this design's actual silicon delays at this process node.

## Run 2: CLOCK_PERIOD = 2.0ns (500MHz)
**Result: Partial pass.** Clean at typical (tt) and fast (ff) corners.
Setup violations remained only at the slow-slow (ss) worst-case corner
(max_ss_100C_1v60, min_ss_100C_1v60, nom_ss_100C_1v60) — the corner
combining slow transistors, high temperature (100°C), and low voltage
(1.60V).

## Run 3: CLOCK_PERIOD = 3.0ns (~333MHz)
**Result: PASSED across all corners.** No setup, hold, max slew, or
max capacitance violations at any PVT corner. DRC, LVS, and Antenna
checks all passed.

## Takeaway
This progression demonstrates that timing closure depends on matching
clock constraints to actual achievable path delays, and that multi-corner
(PVT) analysis matters — a design can pass at nominal conditions while
still failing at worst-case manufacturing/environmental corners.
