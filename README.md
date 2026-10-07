# RTL Bench — Browser-Based Verilog, SystemVerilog & VHDL Simulator with Live Schematic and STA

![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Status](https://img.shields.io/badge/Status-Active-success.svg)
![Platform](https://img.shields.io/badge/Platform-Web-cyan.svg)
![Made with](https://img.shields.io/badge/Stack-Yosys%20%C2%B7%20Icarus%20%C2%B7%20GHDL%20%C2%B7%20nextpnr-blueviolet.svg)

**RTL Bench** is a free, browser-based EDA workbench. Write multi-file SystemVerilog, Verilog or VHDL — or drop a folder of sources — and get an interactive gate-level schematic, real simulation with waveform cursors, structural CDC and RDC hazard analysis, per-cell static timing analysis on open-source PDKs, and one-click FPGA bitstreams, all without a single install.

**No license. No setup. No account. Open the URL and design a chip.**

🔗 **[Launch RTL Bench →](https://rtlbench.com)**

---

## 🖼️ At a glance

| | |
|---|---|
| **Synthesis** | Yosys · multi-file · hierarchy-preserving · flat/techmapped modes |
| **Simulation** | Icarus Verilog · auto-generated testbenches · custom `tb_*.sv` support |
| **Verification** | Structural CDC analyzer · Structural RDC analyzer · lint warnings |
| **Timing** | Real Liberty-table STA on SkyWater sky130, GF180MCU, IHP SG13G2 |
| **FPGA** | Gowin Tang Nano 9K / 20K · Lattice iCE40 UP5K · WebSerial flashing |
| **Languages** | SystemVerilog · Verilog · VHDL (via GHDL) · mixed-language projects |

---

## ✨ Features

### Synthesis & Visualization

- **Multi-file projects.** Drop a folder of `.sv` / `.v` / `.vhd` sources and data files. Relative paths are preserved so `$readmemh("dv/hex/fw.hex")` finds its files.
- **Hierarchy-preserving synthesis.** Submodule instances render as encapsulated blocks with named port pins. Toggle **Hierarchy** vs **Flat** from the same compile.
- **Semantic LOD zooming.** No dropdowns. Zoom level cross-fades between macro block diagrams and detailed gate schematics.
- **Vector bus collapsing.** Bit-blasted Yosys nets are regrouped into single vector wires with IEEE `/W` slash notation and width labels.
- **Fused constant comparators.** `== 8'hA5` renders as a single comparator cell with the value fused into an adjacent chip.
- **Orthogonal Manhattan routing** via Eclipse Layout Kernel (ELK).
- **Click-to-trace nets.** Every wire is interactive. Click any net to highlight its full routed path.
- **Vector SVG export.** Export any view as a clean, resolution-independent SVG.

### Simulation & Verification

- **Built-in waveform viewer** with A/B measurement cursors, delta readout, and derived frequency.
- **Auto-generated testbenches.** No TB? The backend reads the top module's ports with Yosys and generates one: reset release, warmup cycle, 60 random stimulus cycles, drain.
- **Bring your own testbench.** Drop `tb_yourdesign.sv` next to your design — auto-detected by filename convention or `$dumpvars` signature, excluded from synthesis/STA/FPGA, run in simulation.
- **Structural Clock Domain Crossing (CDC) analysis.** Back-traces every register clock boundary. Flags combinational hazards and missing synchronizers. Verifies multi-stage flip-flop chains. Full report page with a domain interaction matrix.
- **Structural Reset Domain Crossing (RDC) analysis.** Catches the same-clock / different-reset bugs that CDC tools miss. Traces reset nets to their source, verifies 2-FF reset synchronizers, flags combinational and soft resets.
- **Bidirectional cross-probing.** Hover a signal in the editor → its waveform trace highlights. Double-click to pin. Click a cell in the schematic → its source line highlights.

### Timing & Analysis

- **Gate-level STA** with real Liberty tables. Yosys `techmap` decomposes to real gates, then a per-cell walk reports critical path delay, slack, and top-K timing paths.
- **Three open-source PDKs.** SkyWater sky130 (130 nm), GlobalFoundries GF180MCU (180 nm), IHP SG13G2 (130 nm BiCMOS). Corner, cell delay, and load model per-PDK.
- **Per-path breakdown.** Every timing path shows a cell-by-cell delay table with cumulative arrival times.
- **Stale-result guard.** Change the source → the STA chip shows a yellow "stale" indicator until you re-run.

### FPGA

- **One-click bitstream generation** for Gowin Tang Nano 9K, Tang Nano 20K, and Lattice iCE40 UP5K.
- **Auto-pin assignment** with conflict recovery. If nextpnr rejects a pin, the flow retries with an alternative.
- **WebSerial flashing** directly from the browser. No `openFPGALoader`, no vendor toolchain.
- **Post-PnR metrics.** LUT usage, FF count, BRAM usage, and Fmax reported from the actual place-and-route log.

---

## 🏗️ The pipeline

Every design runs through the same open-source stack professionals use:
