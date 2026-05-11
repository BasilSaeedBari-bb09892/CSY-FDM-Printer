# Circumferential Synchronous Y-Axis FDM 3D Printer (CSY-Printer)

**Project Title:** Design and Fabrication of a Compact FDM 3D Printer Employing a Circumferential Single-Motor Synchronous Dual-Drive Y-Axis Architecture  
**Platform:** HOIISP — Habib Open Innovation & Independent Study Platform  
**Institution:** Habib University, Engineering & Computer Science Division  
**Author:** Basil Saeed Bari (BB09892) — Computer Engineering  
**Contact:** bb09892@st.habib.edu.pk  
**Status:** Active — In Development  
**Build Volume:** 100 × 100 × 100 mm  
**Firmware:** Klipper  
**Budget:** ≤ Rs 15,000 (components only)

---

## What This Project Is

This repository documents the design, fabrication, and characterisation of a compact FDM (Fused Deposition Modelling) 3D printer that implements a novel Y-axis drive strategy: the **Circumferential Synchronous Y-axis (CSY)** architecture.

In a standard bed-slinger 3D printer, the heated bed is pushed along the Y-axis by a single belt anchored to one side of the bed carriage. The unsupported opposite side of the bed is free to lag behind under inertial load, introducing torsional racking — an angular error that worsens at higher print speeds and degrades dimensional accuracy and surface quality.

The CSY mechanism routes a **single continuous belt circumferentially around the outer perimeter of the printer frame**, coupling both lateral edges of the bed carriage to the same drive motor simultaneously. Because the belt path is symmetric and continuous, both sides of the bed are driven in perfect mechanical synchrony without requiring a second motor, crossed belt geometry, or custom firmware kinematics.

This achieves the mechanical benefit of a dual-Y-motor drive system at the cost of a single motor and a longer belt — and without the kinematic complexity of CoreXY.

---

## The CSY Belt Mechanism — How It Works

```
         ┌──────────────────────────────────────────┐
         │  ╔══════════════════════════════════════╗ │
  REAR   │  ║        X-AXIS GANTRY BAR             ║ │
         │  ╚══════════════════════════════════════╝ │
         │                                           │
  BELT ──┼──→ [CORNER BEARING] ──→ right outer rail  │
  PATH   │                         ↓                 │
         │                    [CORNER BEARING]        │
         │                         ↓                 │
  FRONT  │    BED CARRIAGE ←──────────────────────── │
  (belt attaches here, left AND right edge of bed)
         │                                           │
         │  [MOTOR + DRIVE PULLEY] — rear centre     │
         └──────────────────────────────────────────┘
```

- **One NEMA 17 motor** drives the entire Y-axis.
- The belt runs along the **outside** of the frame, turning at each corner on a flanged bearing or small idler pulley.
- The belt attaches to **both lateral edges** of the bed carriage — the left attachment point on the belt's left pass, the right on the belt's right pass.
- Turning the motor clockwise advances both attachment points by the same distance simultaneously.
- No belt crossover. No second motor. No software synchronisation.

---

## Key Differences from Standard Architectures

| Property | Bed-Slinger (Standard) | CoreXY | CSY (This Project) |
|---|---|---|---|
| Y-axis motors | 1 | 2 (shared with X) | 1 |
| Belt anchoring | Single-sided | N/A (no moving bed) | Dual-sided, synchronous |
| Y torsional racking | Present | N/A | Eliminated (in principle) |
| Belt path complexity | Simple | Crossed | Circumferential (no cross) |
| Firmware kinematics | Standard | CoreXY transform required | Standard (single Y axis) |
| Cost premium over bed-slinger | None | High (2 motors, complex frame) | Low (longer belt + idlers) |
| Moving mass on Y | Bed + carriage | Toolhead only | Bed + carriage |

---

## Repository Structure

```
.
├── cad/
│   ├── frame/                  # Outer frame extrusion layout
│   ├── belt_path/              # CSY belt routing geometry (FreeCAD)
│   ├── brackets/               # Printed corner brackets, motor mounts, carriage
│   └── exports/                # STEP and STL exports for all parts
├── firmware/
│   └── printer.cfg             # Klipper configuration file
├── analysis/
│   └── force_analysis.md       # Static force model for CSY belt attachment points
├── data/
│   ├── characterisation/       # Y-axis parallelism CSV data
│   └── prints/                 # Benchmark print photos
├── docs/
│   ├── BOM.csv                 # Full Bill of Materials with sources and costs
│   ├── BUILD.md                # Step-by-step build guide
│   └── BELT_ROUTING.md         # Detailed belt path setup guide
├── project.md                  # HOIISP submission file
└── README.md                   # This file
```

---

## Hardware Overview

| Component | Specification | Source |
|---|---|---|
| Frame | 20×20 mm V-slot aluminium extrusion | Karachi local market |
| Y-axis motor | NEMA 17, 1.8°, 40 N·cm | Karachi electronics market |
| X-axis motor | NEMA 17, 1.8°, 40 N·cm | Karachi electronics market |
| Z-axis motor | NEMA 17, 1.8°, 40 N·cm | Karachi electronics market |
| Belt | GT2, 2 mm pitch, 6 mm wide | Karachi electronics market |
| Corner idlers | F623 flanged bearings or 608 + printed housing | Karachi electronics market |
| X-axis rails | 8 mm smooth rods + LM8UU bearings | Karachi electronics market |
| Z-axis | T8 lead screw (2 mm pitch) + brass nut | Karachi electronics market |
| Hotend | E3D V6 clone, all-metal, 0.4 mm nozzle | Karachi electronics market |
| Extruder | Bowden drive (reduced moving mass) | Karachi electronics market |
| Heated bed | 100×100 mm PCB bed, 24 V | Karachi electronics market |
| Controller | RAMPS 1.4 + Arduino Mega + A4988 drivers | Karachi electronics market |
| PSU | 24 V / 10 A switching supply | Karachi electronics market |
| Firmware host | Repurposed Intel Atom/Celeron laptop (Klipper) | Sher Shah market, Karachi |

> The RAMPS 1.4 controller is used in this project to isolate the mechanical variable (CSY architecture) from the electronics variable. The companion project — [ESP32-S3 CNC Motherboard](https://github.com/BasilSaeedBari/esp32s3-cnc-controller) — develops a custom control board that may replace RAMPS in a future revision of this printer.

---

## Firmware

This printer runs **Klipper** firmware. Klipper runs the motion planning on a host computer (repurposed laptop) and sends low-level step/direction commands to the RAMPS 1.4 microcontroller over USB serial.

The CSY Y-axis requires **no custom kinematics** in Klipper. Because both sides of the bed are mechanically synchronised by the belt, Klipper sees a standard single-stepper Y-axis. The `printer.cfg` in this repository contains the complete configuration including:

- Stepper pin assignments and microstepping
- Motion limits (100 mm travel per axis)
- Extruder, heated bed, and hotend configuration
- PID tuning values (updated after commissioning)
- Bed levelling (manual mesh, 3-point)

---

## Characterisation Methodology

### Y-Axis Parallelism Test

A dial indicator is mounted on the X-gantry and zeroed against the left edge of the bed. The right edge is measured. The printer then performs 500 rapid Y-axis traversals at 50 mm/s and 100 mm/s. Left and right edge heights are re-measured. The procedure is repeated with the belt reconfigured to single-anchor (standard bed-slinger) geometry on the same frame.

Target: ≥ 40% reduction in peak tilt differential with CSY versus single-anchor belt.

### Dimensional Accuracy

Five 20 × 20 × 20 mm calibration cubes printed. X, Y, Z dimensions measured with digital callipers (±0.01 mm). Target: ≤ 0.2 mm mean error per axis, ≤ 0.1 mm standard deviation.

### Print Quality Benchmarks

Standard overhang test (15°–75° in 15° increments) and a retraction/stringing tower, printed at 50 mm/s and 100 mm/s. Visual pass/fail scoring.

---

## Getting Started (Replicating This Build)

```bash
# 1. Clone this repository
git clone https://github.com/BasilSaeedBari/csy-fdm-printer.git

# 2. Read the build guide
cat docs/BUILD.md

# 3. Review the BOM and source components
cat docs/BOM.csv

# 4. Print all plastic parts (STL files in cad/exports/)
# A standard FDM printer with ≥ 120×120 mm bed is sufficient

# 5. Follow BUILD.md section by section
# Frame → Belt path → X-axis → Z-axis → Electronics → Firmware

# 6. Copy firmware/printer.cfg to your Klipper host and edit
# pin assignments to match your RAMPS wiring
```

Full build documentation is in `docs/BUILD.md`. The belt routing setup — the most novel and critical step — is separately documented in `docs/BELT_ROUTING.md`.

---

## Current Status

See `project.md` → Project Updates section for a running log of progress.

---

## Licence

All design files, firmware configurations, documentation, and characterisation data in this repository are released under the **CERN Open Hardware Licence Version 2 – Strongly Reciprocal (CERN-OHL-S v2)** for hardware designs, and the **MIT Licence** for software and configuration files.

You are free to build, modify, and redistribute this design. Attribution appreciated. If you improve the CSY mechanism, please share it back.

---

## Companion Project

This printer project is one of two related HOIISP projects:

- **[ESP32-S3 CNC Motion Controller](https://github.com/BasilSaeedBari/esp32s3-cnc-controller)** — A custom-designed PCB control board built around the ESP32-S3 N16R8, designed as a general-purpose CNC motion controller optimised for FDM 3D printing. Intended as a future replacement for the RAMPS 1.4 board used in this printer.

---

## Acknowledgements

Supervised under the HOIISP Independent Study Programme, Habib University Engineering & Computer Science Division. Lab resources provided by the Habib University Engineering Workshop. Project inspired by and building upon the RepRap open hardware community.
