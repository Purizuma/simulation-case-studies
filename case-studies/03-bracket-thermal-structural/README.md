# Solar Panel Rail Mid-Clamp Bracket — Thermal-Structural Analysis

**Status:** Complete
**Domain:** Coupled Solid Mechanics + Heat Transfer + Thermal Expansion, per ASCE 7 wind load basis
**Tools:** Autodesk Fusion 360 (geometry), COMSOL Multiphysics 5.6

## Problem statement

A rail mid-clamp bracket clamps two adjacent rooftop solar panels at a shared
mounting rail via a T-bolt. The bracket is subjected simultaneously to bolt
preload, wind-induced uplift, and daily thermal cycling from solar radiation
exposure. Objective: evaluate structural adequacy under combined mechanical
and thermal loading, verify mesh-independence, and quantify the relative
sensitivity to wind load vs. thermal cycling.

## Geometry

| Feature | Dimension |
|---|---|
| Foot plate | 30 × 90 × 4 mm |
| Central boss (raised) | 30 × 50 × 6 mm (10 mm total thickness at boss) |
| Bolt hole | Ø8.5 mm through-hole, centered in boss |
| Step transition fillets | R2 mm (stress relief, both sides) |
| Material | Aluminum 6061-T6 |

Geometry built parametrically via the Fusion 360 API, exported as a **SAT**
solid for import into COMSOL (avoids arc-splitting on filleted features —
see the [toolkit gotchas doc](../../../fusion-comsol-automation-toolkit/docs/gotchas.md)).

## Material (custom-defined, not COMSOL's built-in library)

| Property | Symbol | Value | Unit |
|---|---|---|---|
| Density | ρ | 2,700 | kg/m³ |
| Young's modulus | E | 68.9 | GPa |
| Poisson's ratio | ν | 0.33 | — |
| Coeff. of thermal expansion | α | 23.6 × 10⁻⁶ | 1/K |
| Thermal conductivity | k | 167 | W/(m·K) |
| Heat capacity | Cp | 896 | J/(kg·K) |
| Yield strength (reference) | σy | 276 | MPa |

Defined manually rather than via COMSOL's library, since the library dataset
(cryogenic test data, long-term creep, fatigue S-N curves) doesn't match this
bracket's 25–70°C service range.

## Physics & boundary conditions

Three coupled physics interfaces via a Thermal Expansion multiphysics node:
Solid Mechanics, Heat Transfer in Solids, and Thermal Expansion (secant CTE,
298.15 K stress-free reference).

| Boundary Condition | Location | Value / Direction |
|---|---|---|
| Fixed Constraint | Bolt-hole cylindrical wall | Fully rigid (u = 0) |
| Bolt Preload | Top face of boss | 3,000 N, −Z |
| Wind Uplift | Bottom face of foot plate | 2,540 N, +Z |
| Temperature | All exterior boundaries | Parametrized 298.15–343.15 K |
| Strain reference temperature | Volume | 298.15 K (25°C) |

Wind uplift (2,540 N) derives from an ASCE 7-based velocity pressure
(qz ≈ 1.27 kPa; V = 120 mph, Exposure C, Kzt = 1.0, Kd = 0.85) with a
representative uplift coefficient GCrn ≈ 2.0 over a 1.0 m² tributary area.
**Note:** this is a simplified velocity-pressure approach (ASCE 7 Eq. 26.10-1),
not the full rooftop-solar-array provisions of ASCE 7-16/22 Ch. 29.4 —
adequate for demonstrating methodology, not a code-compliance certification.

## Mesh convergence study

Five-level refinement (Normal → Extremely Fine, 4,029 → 115,621 elements) to
verify mesh-independence and distinguish genuine stress concentrations from
boundary-condition artifacts. Raw data: [`data/mesh_convergence.csv`](data/mesh_convergence.csv).

**Result:** fillet stress remains bounded (~65–75 MPa) across all mesh levels
— mesh-independent. Bolt-hole edge stress rises without plateau (200.5 →
251.7 MPa) — a classic re-entrant corner singularity from the idealized rigid
Fixed Constraint, confirmed **not** representative of physical failure risk.

Refined Surface-Maximum evaluation (Extremely Fine mesh):

| Location | Von Mises Stress (MPa) | Safety Factor (σy = 276 MPa) |
|---|---|---|
| Fillet 1 (Y = 19.41 mm) | 84.72 | 3.26 |
| Fillet 2 (Y = 70.59 mm) | 84.92 | 3.25 |
| Bolt-hole edge (singularity, informational only) | 251.70 | 1.10 (not representative) |

Fillet 1/Fillet 2 agree within 0.3%, confirming the expected structural
symmetry of the clamp design.

## Parametric sweep — wind load × thermal cycling

5×5 sweep (25 combinations): wind load factor 0.2–1.0× baseline, ambient
temperature 25–70°C. Raw data: [`data/parametric_sweep.csv`](data/parametric_sweep.csv).

| Wind Load Factor | Fillet Stress @ 25°C (MPa) | Fillet Stress @ 70°C (MPa) | Δ due to ΔT |
|---|---|---|---|
| 0.2 | 16.84 | 16.91 | 0.42% |
| 0.4 | 33.79 | 33.85 | 0.18% |
| 0.6 | 50.73 | 50.80 | 0.14% |
| 0.8 | 67.63 | 67.75 | 0.18% |
| 1.0 | 84.63 | 84.70 | 0.08% |

**Key findings:**
- Stress scales essentially linearly with wind load (5× load → ~5× stress) —
  consistent with linear-elastic behavior
- Thermal cycling across the full 25–70°C range changes peak stress by
  well under 1% — the bracket is largely free to expand thermally since only
  the bolt hole is rigidly constrained
- **Wind loading, not thermal fatigue, governs the structural margin**
- Worst-case combination (wind factor 1.0, T=70°C) gives 84.84 MPa — SF 3.25,
  matching baseline, confirming no unexpected load-interaction effect

## Conclusions

The bracket (30×90×4mm foot, 30×50×6mm boss, R2mm fillets, Al 6061-T6)
provides a **Safety Factor of ~3.25** against yield at the governing fillet
locations under the full combined load case (bolt preload + ASCE 7 wind
uplift + 45°C thermal cycling) — structurally adequate with substantial
reserve capacity. Mesh convergence confirms the result is mesh-independent;
the elevated bolt-hole reading is a modeling artifact of the idealized rigid
constraint, not a physical failure indicator.

## Recommendations for further work

- Replace the idealized rigid Fixed Constraint with a realistic bolted-joint
  model (contact pair with friction, or a Pin/Bolt connector) for a
  physically meaningful bolt-hole stress value
- Extend to fatigue-life assessment (S-N curve, Miner's rule) for long-term
  daily thermal cycling using COMSOL's Fatigue Module
- Validate the simplified ASCE 7 assumption against full rooftop-solar-array
  provisions (ASCE 7-16/22 §29.4) if pursuing code-compliant certification
- Re-run this sweep methodology if a thinner foot plate or smaller fillet
  radius is considered for weight/cost optimization

## Deliverables

- Full engineering report: [`assets/Rail_MidClamp_Bracket_CaseStudy.docx`](assets/Rail_MidClamp_Bracket_CaseStudy.docx)
- Raw mesh convergence and parametric sweep data (CSV, in `data/`)

*(Add CAD renders and stress contour plot images to `assets/` if available —
tracked via Git LFS.)*
