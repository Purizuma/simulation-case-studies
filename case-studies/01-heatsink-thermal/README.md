# Heatsink Thermal Validation — 10W Buck Converter

**Domain:** Steady-state conduction/convection heat transfer
**Tools:** Fusion 360 (geometry), COMSOL Multiphysics 5 (Heat Transfer in Solids/Fluids)

## Problem statement

Straight-fin aluminum heatsink thermal validation for a 10W buck converter
(12V → 5V/2A) used in a solar charge controller application. Target: case
temperature below 85°C across a realistic range of convective heat transfer
coefficients (natural to light forced convection).

## Methodology

1. Heatsink geometry parametrically built in Fusion 360
2. Steady-state heat transfer analysis in COMSOL
3. **Mesh convergence study** — refined mesh until solution changed negligibly
   between levels
4. **Parametric h-sensitivity sweep** — swept convective coefficient across the
   natural-to-forced convection range to bound real-world performance
5. Analytical cross-check against standard fin-efficiency correlations

## Key results

| Convective coefficient (h) | Max temperature | Result |
|---|---|---|
| 10 W/m²K (still air) | 103.8°C | **Fail** — exceeds 85°C target |
| 15 W/m²K (light airflow) | 76.1°C | **Pass** |

**Engineering takeaway:** the design is airflow-dependent — passive natural
convection alone is insufficient; a minimum of light forced airflow (or a
larger fin area) is required to meet the thermal budget with margin.

## Deliverables

- CAD renders (isometric, exploded fin view)
- Mesh wireframe screenshots
- Temperature contour plots at both h values
- Convergence and h-sensitivity charts
- Full 7-page engineering report (`Heatsink_Thermal_CaseStudy.docx`) — available
  in `assets/` once uploaded, or on request

## Assets

*(Upload CAD renders, mesh screenshots, contour plots, and the full .docx report
into `assets/` — tracked via Git LFS.)*
