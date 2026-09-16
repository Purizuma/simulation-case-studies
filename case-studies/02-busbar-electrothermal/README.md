# Copper Busbar Electro-Thermal Analysis — 30A DC Connector

**Domain:** Multiphysics — Electric Currents coupled with Heat Transfer in Solids
(Joule heating)
**Tools:** Fusion 360 (geometry, SAT/ACIS export), COMSOL Multiphysics 5
(Electromagnetic Heating multiphysics node)

## Problem statement

Copper busbar (100 × 30 × 3 mm, two Ø6 mm mounting holes with 1 mm edge fillet)
used as a 30A DC connection in a solar charge controller. Validate resistive
heating under rated current and characterize the current-carrying margin above
rated load.

## Methodology & notable engineering decisions

- Geometry exported as **SAT/ACIS**, not STEP, to avoid arc-splitting on import
  into COMSOL (a common Fusion → COMSOL interoperability issue on filleted/
  curved features)
- Multiphysics coupling: Electric Currents + Heat Transfer in Solids, coupled
  bidirectionally via linearized temperature-dependent resistivity
  (Electromagnetic Heating node)
- **Boundary condition fix:** initial Terminal/Ground BCs placed on the hole
  walls caused current-density singularities (NaN convergence errors) at the
  insulation boundary. Resolved by moving BCs to the end cross-section faces —
  a general lesson for any electro-thermal busbar/connector model
- Mesh convergence study across 5 refinement levels (546 → 21,697 elements),
  confirmed mesh-independent to within 0.68%
- Parametric current sweep from 10A to 450A to find the reliability limit, not
  just the rated-current result

## Key results

| Metric | Result |
|---|---|
| Max temperature at rated 30A | 20.2°C (0.2°C rise above ambient) |
| Total resistive loss at 30A | 0.0136 W |
| Current crowding | Visible near mounting holes, as expected |
| Mesh convergence | Independent to within 0.68% (5 refinement levels) |
| Current-carrying capacity | ~430A before reaching a 50°C reliability threshold |
| Cross-check vs. IPC-2221 | IPC-2221 analytical formula found **non-conservative
  by 3–4×** for this thick solid conductor geometry vs. FEM result |

**Engineering takeaway:** for thick, solid (non-PCB-trace) conductor geometry,
the widely-used IPC-2221 formula significantly underpredicts current-carrying
capacity — full-field FEM thermal analysis reveals substantial unused margin
that a conservative hand-calculation would miss.

## Deliverables

- CAD renders, current density and temperature contour plots
- Mesh convergence and parametric sweep charts
- Full 8-page engineering report (`Busbar_ElectroThermal_CaseStudy.docx`) —
  available in `assets/` once uploaded, or on request

## Assets

*(Upload CAD renders, contour plots, sweep charts, and the full .docx report
into `assets/` — tracked via Git LFS.)*
