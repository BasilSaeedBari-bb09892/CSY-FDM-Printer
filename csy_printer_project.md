# Project Title

`Design and Fabrication of a Compact FDM 3D Printer Employing a Circumferential Single-Motor Synchronous Dual-Drive Y-Axis Architecture`

---

## GitHub Repository

`https://github.com/BasilSaeedBari/csy-fdm-printer`

---

## Team Members

| Full Name | Student ID | GitHub Username | Habib Email | Program | Year | Role |
|---|---|---|---|---|---|---|
| Basil Saeed Bari | BB09892 | BasilSaeedBari | bb09892@st.habib.edu.pk | CE | 4 | Lead / Principal Investigator |

---

## Abstract

Fused Deposition Modelling (FDM) 3D printers are the dominant platform for desktop additive manufacturing, yet their motion systems represent engineering trade-offs that are rarely revisited at the student or hobbyist level. The most widely deployed kinematic architecture — the bed-slinger — advances the heated bed along the Y-axis using a single belt anchored to one side of the bed carriage, leaving the opposite side of the bed unsupported and susceptible to torsional loading, racking, and geometric error at higher print speeds. The alternative, CoreXY, eliminates this problem but requires two motors, a crossed belt path, complex firmware kinematics, and precise belt tension balancing — substantially increasing cost and calibration difficulty.

This project proposes and physically validates a novel Y-axis belt routing strategy designated the Circumferential Synchronous Y-axis (CSY) architecture. In the CSY design, a single continuous belt is routed circumferentially around the outer perimeter of the printer frame, looping back on itself so that both lateral sides of the bed carriage are driven simultaneously by a single stepper motor. This eliminates the torsional asymmetry inherent to the standard bed-slinger without requiring a second motor or crossed belt geometry.

The project will produce a complete, functional 100 × 100 × 100 mm build-volume FDM 3D printer implementing the CSY mechanism, designed for full reproducibility using components available in the Karachi local market and the Habib University Engineering Workshop. The printer will be characterised against an equivalent single-belt bed-slinger reference machine for geometric accuracy, Y-axis parallelism under dynamic load, and print quality across a range of speeds.

Successful completion demonstrates the viability of the CSY architecture as a lower-cost, mechanically superior alternative to both conventional bed-slingers and CoreXY systems for compact desktop FDM machines.

---

## Problem Statement

Standard cartesian FDM printers that translate the heated bed along the Y-axis — commonly called bed-slingers — anchor the drive belt to a single point on one lateral edge of the bed carriage. This creates a moment arm between the drive point and the unsupported opposite edge. Under the dynamic inertial loads of high-speed printing, this moment produces measurable torsional racking: the unsupported edge of the bed lags behind the driven edge, introducing Y-axis angular error that manifests in printed parts as layer misalignment, reduced dimensional accuracy on the Y-axis, and surface quality degradation at elevated speeds.

The canonical engineering solution — using two independently driven Y-axis motors (one per side) — requires precise synchronisation, doubles the motor count and driver cost, and adds firmware complexity. CoreXY systems solve the structural problem differently but introduce their own calibration burden and cost premium.

No widely documented, validated solution exists that achieves synchronous dual-side Y-axis drive from a single motor using a simple, non-crossed belt path. This project identifies, designs, and physically demonstrates such a solution: a circumferentially routed belt that couples both lateral sides of the bed to a single drive motor without crossover geometry. The specific problem being solved is: can a single-motor, circumferential belt path mechanically synchronise both sides of a moving bed platform and measurably reduce torsional racking error compared to a single-anchor belt at the 100 mm/s print speed range, within a sub-Rs 15,000 independently funded component budget?

---

## Domain & IEEE Alignment

**Primary Domain:** *(select one by replacing `[ ]` with `[x]`)*
- [ ] Electrical Engineering
- [ ] Computer Science
- [x] Computer Engineering
- [ ] Mechatronics / Robotics
- [ ] Telecommunications
- [ ] Power Systems
- [ ] Signal Processing
- [ ] Other: _______________

**Sub-Field / Specialization:**  
`Additive Manufacturing Systems, Precision Mechatronics, Embedded Motion Control`

**Relevant IEEE Technical Society:**  
`IEEE Robotics and Automation Society; IEEE Industrial Electronics Society`

**Applicable IEEE Standard(s), if any:**  
`None directly applicable. Design references ASME Y14.5 GD&T conventions for dimensional tolerance reporting and IPC-2221 mechanical design practices informally.`

---

## Objectives

1. To design, model, and validate the Circumferential Synchronous Y-axis (CSY) belt routing geometry using free CAD tools, confirming mechanical feasibility and belt path continuity prior to physical construction.
2. To fabricate a structurally complete 100 × 100 × 100 mm build-volume FDM 3D printer implementing the CSY Y-axis mechanism, using components sourced exclusively from the Karachi local market and the Habib University Engineering Workshop.
3. To characterise Y-axis parallelism error under dynamic load by measuring bed-carriage lateral tilt at 50 mm/s and 100 mm/s using a dial indicator, comparing the CSY machine against a single-anchor belt reference configuration on the same frame.
4. To produce a minimum of three benchmark print sets (XY dimensional accuracy cube, overhang test, and retraction tower) at 50 mm/s and 100 mm/s, scoring output against ISO 2768 fine tolerance class criteria.
5. To publish a complete, reproducible design package — including CAD files, belt routing diagrams, firmware configuration, and build instructions — to the project GitHub repository under an open licence, enabling independent replication by future students.

---

## Methodology

### Design & Simulation Phase

The CSY belt path will be modelled in FreeCAD. The key geometric constraint is that the belt must loop around the outer perimeter of the frame and couple to both lateral sides of the bed carriage with equal and opposite tension vectors, such that both sides advance synchronously when the drive motor turns. The path will be laid out in a 2D sketch first, confirming belt length, pulley placement, and the absence of crossing geometry.

A simplified static force analysis will be performed: given a 500 g bed carriage mass, 2 m/s² maximum acceleration, and symmetric belt attachment points at the carriage edges, the model will confirm that torsional moment on the bed is reduced to zero under ideal conditions and quantify residual error introduced by belt elasticity and pulley misalignment tolerances.

Firmware kinematic configuration will use Klipper on a repurposed laptop (Intel Atom or Celeron, sourced from Sher Shah market). The CSY Y-axis maps directly to a single stepper axis in Klipper — no custom kinematics module is required, as the synchronous coupling is achieved mechanically rather than in software.

### Hardware / Software Implementation Phase

**Frame:** Aluminium extrusion profile (20 × 20 mm V-slot or equivalent, locally sourced) forming a rigid rectangular outer perimeter. The circumferential belt path runs along this outer perimeter, riding on 608-type or F623 flanged bearings at each corner.

**Y-Axis Drive:** A single NEMA 17 stepper motor mounted at the rear centre of the frame drives a GT2 pulley. The continuous GT2 belt runs: drive pulley → along rear outside of frame → corner bearing → along right outside of frame → corner bearing → along front outside of frame → corner bearing → along left outside of frame → back to drive pulley. Two belt attachment points on the bed carriage, one per lateral side, are engaged by the belt as it passes along the front and rear frame rails (or equivalent geometry confirmed during CAD phase).

**X-Axis:** A single NEMA 17 motor drives a GT2 belt carriage carrying the hotend assembly along two smooth rods or V-slot rails spanning the X dimension. Bowden extruder to minimise moving mass.

**Z-Axis:** Lead screw (T8 × 2 mm pitch) driven by a NEMA 17 motor, lifting the X-gantry assembly. Two smooth rod guides for parallelism.

**Hotend:** E3D V6 clone or equivalent all-metal hotend, 0.4 mm nozzle, 24 V.

**Heated Bed:** 100 × 100 mm PCB heated bed, 24 V.

**Controller:** RAMPS 1.4 + Arduino Mega running Klipper (USB serial to host laptop). This is intentionally a commodity controller to isolate the mechanical variable (CSY architecture) from the electronics variable, which is addressed in the companion ESP32-S3 motherboard project.

**Firmware:** Klipper. Configuration file will be version-controlled in this repository.

### Testing & Validation Phase

**Y-axis parallelism test:** A dial indicator mounted on the X-gantry measures bed surface tilt (left edge vs. right edge) at rest and after 500 rapid Y-axis traversals at 50 mm/s and 100 mm/s. Results compared against the same frame reconfigured with a single-anchor belt.

**Dimensional accuracy:** Print a 20 × 20 × 20 mm calibration cube. Measure X, Y, Z dimensions with digital callipers to ±0.01 mm resolution. Target: ≤ 0.2 mm error on all axes.

**Print quality benchmarks:** Standard overhang test (15°–75°) and retraction/stringing tower at 50 and 100 mm/s. Visual scoring against community reference prints.

**Repeatability:** Print the calibration cube five times without re-homing; measure dimensional variance. Target: ≤ 0.1 mm standard deviation across all axes.

---

## Work Breakdown Structure (WBS)

| Milestone # | Milestone Name | Key Deliverables | Start Date | End Date | Status |
|---|---|---|---|---|---|
| M1 | Design & Simulation | CSY belt path CAD model, force analysis document, firmware skeleton configuration | 2025-06-01 | 2025-06-21 | Not Started |
| M2 | Component Procurement | All mechanical, electrical, and fastener components received and inventoried | 2025-06-15 | 2025-06-28 | Not Started |
| M3 | Frame & Motion Assembly | Assembled frame with CSY belt path, X-axis, and Z-axis functional; manual axis movement verified | 2025-06-29 | 2025-07-19 | Not Started |
| M4 | Electronics & Firmware Bring-up | RAMPS wired, Klipper configured, all axes homing correctly; heated bed and hotend reaching target temperatures | 2025-07-20 | 2025-08-02 | Not Started |
| M5 | Calibration & Characterisation | Y-axis parallelism data collected; benchmark prints completed; calibration cube results logged | 2025-08-03 | 2025-08-23 | Not Started |
| M6 | Documentation & Repository Finalisation | Complete CAD, BOM, firmware config, test data, and build guide committed to GitHub | 2025-08-24 | 2025-08-31 | Not Started |

**Estimated Total Duration:** `13 weeks`

---

## Resource Management Matrix

| Resource | Lab / Location | Estimated Hours | Purpose in Project | Required From | Required Until |
|---|---|---|---|---|---|
| Digital Callipers (±0.01 mm) | Engineering Workshop | 4 hrs | Dimensional accuracy measurements on printed benchmarks | 2025-08-03 | 2025-08-23 |
| Dial Indicator + Magnetic Base | Engineering Workshop | 6 hrs | Y-axis parallelism characterisation under dynamic load | 2025-08-03 | 2025-08-16 |
| Bench Drill Press | Engineering Workshop | 3 hrs | Frame bracket and motor mount hole drilling | 2025-07-05 | 2025-07-12 |
| Bench Vice + Hand Tools | Engineering Workshop | 10 hrs | Frame assembly, belt tensioning, axis alignment | 2025-06-29 | 2025-08-02 |
| Existing Lab 3D Printers (×2) | Engineering Workshop | 20 hrs | Printing plastic brackets, motor mounts, carriage parts, and belt guides | 2025-06-15 | 2025-08-10 |
| Digital Multimeter | Electronics Lab | 4 hrs | Wiring continuity checks, thermistor resistance verification | 2025-07-20 | 2025-08-02 |
| Variable DC Bench Power Supply | Electronics Lab | 8 hrs | Pre-wiring electronics bench testing before installation in frame | 2025-07-15 | 2025-08-02 |

> If you require access to Tier 2/3 equipment (Lathe, Drill Press, Milling Machine, PCB CNC), confirm below:

- [x] I have read Schedule A of the Terms & Conditions and will complete all required safety training before using listed Tier 2/3 equipment.

---

## Risk Assessment

| Risk | Likelihood (H/M/L) | Impact (H/M/L) | Mitigation Strategy |
|---|---|---|---|
| CSY belt path geometry produces unequal tension on the two sides of the bed carriage due to belt elasticity and corner-bearing friction, failing to eliminate racking | M | H | Design belt attachment points with individual tension-adjustment slots. If unequal tension is confirmed by dial indicator, add an idler tensioner on the low-tension side. Document the residual error as a quantified finding rather than a project failure. |
| Locally sourced aluminium extrusion is not true (twisted or bowed), introducing frame-level squareness error that confounds Y-axis measurements | M | M | Inspect extrusion with a straight-edge before purchase; select the straightest available. Design corner brackets with slotted holes to allow squareness adjustment during assembly. |
| GT2 belt length mismatch for the circumferential path requiring a custom splice | L | M | Calculate belt length exactly from CAD before ordering; order 20% excess. Test belt clip/splice joint strength before installation. |
| Lab 3D printers unavailable during bracket printing phase (booked or broken) | M | M | Begin bracket design and slicing during M1; queue prints at earliest opportunity. Identify off-campus FDM print service (PrintKarachi or similar) as backup. |
| Klipper host (repurposed laptop) hardware failure | L | H | Image the SD card after initial Klipper setup. Identify a backup laptop before the firmware bring-up milestone. |

---

## Success Metrics

| Metric | Target Value | Measurement Method |
|---|---|---|
| 1. Y-axis tilt differential (CSY vs. single-anchor belt, after 500 traversals at 100 mm/s) | ≥ 40% reduction in peak tilt angle | Dial indicator on bed surface, left vs. right edge, recorded before and after traversal test |
| 2. Calibration cube dimensional error (X, Y, Z) | ≤ 0.2 mm on all three axes | Digital callipers, mean of 5 prints |
| 3. Calibration cube dimensional repeatability | ≤ 0.1 mm standard deviation across 5 prints on each axis | Digital callipers, standard deviation calculation |
| 4. Maximum validated print speed with acceptable quality | ≥ 80 mm/s (perimeters) | Visual quality pass/fail on overhang and retraction benchmark prints |
| 5. Component cost (excluding tools and university equipment) | ≤ Rs 15,000 | Itemised BOM with receipts committed to repository |
| 6. Build reproducibility | Complete build by a second person from documentation alone, without author assistance | Documented independent replication attempt or structured walkthrough review |

---

## Project Updates

### Update Log

#### 2025-06-01 — Project Initiated
Initial CAD sketching of CSY belt path begun. Frame outer dimensions fixed at 220 × 220 mm (outer) to accommodate 100 × 100 mm build volume with adequate belt routing clearance. Component sourcing in progress.

---

## Data & Documentation Plan

- Belt path geometry: FreeCAD `.FCStd` source files in `/cad/`
- Exported STEP and STL files for all printed parts in `/cad/exports/`
- Force analysis worksheet in `/analysis/`
- Firmware Klipper configuration (`printer.cfg`) in `/firmware/`
- Y-axis parallelism test data as `.csv` in `/data/characterisation/`
- Benchmark print photos in `/data/prints/`
- Bill of Materials as `.csv` in `/docs/`
- Build guide as `BUILD.md` in root

**Repo structure commitment:** I understand that the HOIISP project page links directly to this repository. The repo should be organised and have a readable top-level README.

- [x] Confirmed.

---

## Budget Estimate

| Item | Source | Estimated Cost (PKR) |
|---|---|---|
| Aluminium V-slot extrusion 20×20 mm (4× 300 mm lengths) | Local hardware, Karachi | 800 |
| NEMA 17 stepper motors × 3 (X, Y, Z) | Electronics market, Karachi | 2,400 |
| GT2 belt (2 m) + GT2 pulleys × 4 + idler bearings × 6 | Electronics market, Karachi | 1,200 |
| 8 mm smooth rods × 4 (X and Z axes) + LM8UU linear bearings × 8 | Electronics market, Karachi | 1,500 |
| T8 lead screw + brass nut (Z axis) | Electronics market, Karachi | 600 |
| E3D V6 clone hotend + Bowden extruder assembly | Electronics market, Karachi | 1,200 |
| 100×100 mm PCB heated bed + thermistor | Electronics market, Karachi | 800 |
| RAMPS 1.4 + Arduino Mega + A4988 drivers × 4 | Electronics market, Karachi | 1,800 |
| 24 V / 10 A PSU | Electronics market, Karachi | 1,200 |
| Assorted fasteners (M3, M4, M5), nuts, washers | Local hardware, Karachi | 500 |
| PLA filament (1 kg, for printed brackets and mounts) | Local market, Karachi | 1,500 |
| Repurposed laptop (Atom/Celeron) for Klipper host | Sher Shah market, Karachi | 7,000 |
| Miscellaneous (wire, connectors, endstops, fan) | Electronics market, Karachi | 800 |
| | **Total Estimated:** | **21,300** |

> Note: Repurposed laptop is a one-time asset shared with or replaced by the companion ESP32-S3 motherboard project. Effective printer-specific cost without the laptop is approximately Rs 14,300.

---

## References

[1] J. M. Pearce, C. Blair, K. Laciak, R. Andrews, A. Nosrat, and I. Zelenika, "3-D printing of open source appropriate technologies for self-directed sustainable development," *Journal of Sustainable Development*, vol. 3, no. 4, pp. 17–29, 2010.

[2] RepRap Project, "CoreXY kinematic system," RepRap Wiki. [Online]. Available: https://reprap.org/wiki/CoreXY

[3] A. Gebhardt, *Understanding Additive Manufacturing: Rapid Prototyping, Rapid Tooling, Rapid Manufacturing*. Munich: Carl Hanser Verlag, 2011.

[4] Klipper3d Project, *Klipper Firmware Documentation*, 2024. [Online]. Available: https://www.klipper3d.org/

[5] NEMA ICS 16-2001, *Motion/Position Control Motors and Controls*, National Electrical Manufacturers Association, 2001.

[6] R. Williams, "Belt-driven motion systems: tension, compliance, and positional error," *Precision Engineering*, vol. 14, no. 2, pp. 67–78, 1992.

---

## Declaration

- [ ] I confirm that the information in this `project.md` is accurate and complete.
- [ ] I have read and agree to the HOIISP Terms & Conditions in full.
- [ ] I confirm that all listed team members are aware of this submission and have agreed to participate.
- [ ] I confirm that at least one team member has committed to this repository using a `@st.habib.edu.pk` Git email address.
- [ ] I understand that approval of this submission grants access only to the resources explicitly listed in the Resource Management Matrix.
- [ ] I understand that I must push a `project.md` update at least once every two weeks to maintain Active project status and lab access.
- [ ] I accept that all content parsed from this repository will be publicly visible on the HOIISP platform.
- [ ] I confirm this repository is set to **Public** visibility on GitHub (HOIISP cannot read private repositories).

---

## For Office Use Only
> *Do not fill in this section.*

| Field | Value |
|---|---|
| HOIISP Submission ID | |
| GitHub Verification Status | |
| Assigned Project Slug | |
| Review Outcome | Approved / Rejected / Revisions Required |
| Review Notes | |
| Admin | |
| Date of Decision | |
| Resource Request Forwarded | |
| Lab Access Activated | |
