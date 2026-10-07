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



```
┌──────────────┐    ┌──────────┐    ┌─────────┐    ┌──────────┐
│  Upload RTL  │──▶│  Yosys   │──▶│   ELK   │──▶│ Analyze  │
│  (multi-file)│    │ elaborate│    │ layout  │    │ + export │
└──────────────┘    └──────────┘    └─────────┘    └──────────┘
                          │
                          ├──▶ Icarus  ──▶ VCD  ──▶ Waveform viewer
                          ├──▶ GHDL    ──▶ (VHDL sim)
                          ├──▶ CDC engine   ──▶ Hazard report
                          ├──▶ RDC engine   ──▶ Hazard report
                          ├──▶ Liberty walk ──▶ STA report
                          └──▶ nextpnr      ──▶ FPGA bitstream ──▶ WebSerial
```

---

## 🚀 Getting started (frontend only)

This repository contains the **frontend UI and landing page** for RTL Bench.

### Prerequisites

- Node.js (v18 or higher)
- npm or yarn

### Local development

```bash
# 1. Clone
git clone https://github.com/Raghavan-04/rtl-bench-public.git
cd rtl-bench-public

# 2. Install
npm install

# 3. Run the dev server
npm run dev

# 4. Open http://localhost:5173
```

> **⚠️ Backend not included.** The synthesis, simulation, STA, CDC/RDC, and FPGA engines are proprietary and not part of this repo. The UI loads locally, but API calls will fail. To use the full tool, visit **[rtlbench.com](https://rtlbench.com)**.

---

## 🧩 Open-core model

RTL Bench operates on an **open-core** model:

| Part | Status | What it includes |
|---|---|---|
| **Frontend** (this repo) | Open source, MIT | UI, landing page, schematic renderer, waveform viewer, all HTML/CSS/JS |
| **Backend engine** | Proprietary | Yosys orchestration, Icarus/GHDL invocation, Liberty STA, CDC/RDC analyzers, FPGA build flow |

We welcome contributions to the frontend, UI/UX, and client-side tooling. For backend integration or commercial licensing, contact **contact@rtlbench.com**.

---

## 🛠️ Tech stack

**Open-source engines powering the backend:**

- [Yosys](https://yosyshq.net/yosys/) — RTL synthesis
- [Icarus Verilog](https://steveicarus.github.io/iverilog/) — SystemVerilog simulation
- [GHDL](https://github.com/ghdl/ghdl) — VHDL simulation
- [nextpnr](https://github.com/YosysHQ/nextpnr) — FPGA place & route
- [Eclipse Layout Kernel](https://www.eclipse.org/elk/) — schematic layout

**Open-source PDKs:**

- [SkyWater sky130](https://github.com/google/skywater-pdk)
- [GlobalFoundries GF180MCU](https://github.com/google/gf180mcu-pdk)
- [IHP SG13G2](https://github.com/IHP-GmbH/IHP-Open-PDK)

**Frontend:**

- Vanilla JavaScript (no framework)
- FastAPI (backend serving, not in this repo)
- ELK.js (client-side layout)

---

## 👥 Who it's for

- **Students & self-learners.** Learn RTL without fighting toolchains. Paste SystemVerilog, see gates, run a testbench, watch the waveform.
- **Hardware engineers.** Quick sanity checks. Real sky130/GF180/IHP cell timing, per-cell critical-path breakdown, CDC/RDC audits, SVG export for design reviews.
- **Researchers & educators.** Share a link instead of a repo. Every design loads live synthesis, STA, CDC/RDC verification, and a waveform — reproducible from any browser.

---

## 🗺️ Roadmap

- [ ] Public design permalinks (`rtlbench.com/d/<id>`)
- [ ] `localStorage` autosave + "Recent designs"
- [ ] Formal verification (SymbiYosys integration)
- [ ] Verilator backend for faster simulation
- [ ] Design gallery with tags (`fsm`, `cpu`, `dsp`, `cdc`)
- [ ] SVA assertion surfacing
- [ ] On-prem enterprise deployment

---

## 🤝 Contributing

Contributions are welcome — bug reports, feature requests, UI improvements, and pull requests.

1. Fork the repo
2. Create a branch (`git checkout -b feature/amazing-feature`)
3. Commit (`git commit -m 'Add amazing feature'`)
4. Push (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please read `CONTRIBUTING.md` for code of conduct and PR guidelines.

**Good first issues** are tagged in the issue tracker. UI polish, accessibility improvements, and documentation fixes are all great places to start.

---

## 📄 License

MIT License — see [LICENSE](LICENSE).

*The MIT License applies only to the files included in this repository. The backend synthesis engine remains the intellectual property of the author.*

---

##  Author

**Raghavan S U**

- 🌐 [rtlbench.com](https://rtlbench.com)
- 💼 [LinkedIn](https://www.linkedin.com/in/raghavan-su-04r/)
- 🐙 [GitHub](https://github.com/Raghavan-04/)
- ✉️ [contact@rtlbench.com](mailto:contact@rtlbench.com)

Built for the open-source hardware community.

---

##  Acknowledgments

- [Yosys Open SYnthesis Suite](https://yosyshq.net/yosys/)
- [Icarus Verilog](https://steveicarus.github.io/iverilog/)
- [GHDL](https://github.com/ghdl/ghdl)
- [nextpnr](https://github.com/YosysHQ/nextpnr)
- [Eclipse Layout Kernel](https://www.eclipse.org/elk/)
- The open PDK teams at SkyWater, GlobalFoundries, and IHP

---

 **If RTL Bench helped you, consider starring the repo — it helps other engineers find it.**
 
