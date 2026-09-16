# Simulation Case Studies — Fusion 360 & COMSOL Multiphysics

Portfolio of multiphysics engineering simulation case studies: thermal, electro-thermal,
and thermal-structural analysis for power electronics and renewable energy hardware.

Workflow: **Fusion 360 (API-scripted geometry) → COMSOL Multiphysics (physics setup,
mesh convergence, parametric study) → analytical cross-validation → engineering report.**

Full client-facing reports and freelance simulation engineering services available via
[Upwork](https://www.upwork.com/freelancers/antoniusp).

## Case studies

| # | Case study | Domain | Key result |
|---|---|---|---|
| 01 | [Heatsink thermal validation](case-studies/01-heatsink-thermal/) | Steady-state heat transfer | Fails at h=10 W/m²K (103.8°C), passes at h=15 W/m²K (76.1°C) |
| 02 | [Copper busbar electro-thermal](case-studies/02-busbar-electrothermal/) | Joule heating (Electric Currents + Heat Transfer) | Rated 30A → 20.2°C; tolerates ~430A before 50°C reliability threshold |
| 03 | [Solar bracket thermal-structural](case-studies/03-bracket-thermal-structural/) | Structural + thermal (Solid Mechanics + Heat Transfer + Thermal Expansion), ASCE 7 wind load | SF ≈ 3.25 vs. yield at governing fillets; wind load dominates, thermal cycling contributes <0.2% |

## Why this matters (methodology, not just geometry)

CAD geometry alone doesn't validate a design. Every case study here includes:

- Mesh convergence study (multiple refinement levels, quantified % change)
- Parametric sensitivity sweep (not just a single operating point)
- Cross-validation against a classical/analytical formula, with discrepancy explained
- Full engineering narrative suitable for a design review

## Repository structure

```
case-studies/
  01-heatsink-thermal/
    README.md          # summary, methodology, key findings
    assets/            # renders, mesh plots, contour plots (Git LFS)
  02-busbar-electrothermal/
  03-bracket-thermal-structural/
```

Heavy files (`.docx`, `.step`, `.sat`, `.mph`, images) are tracked via **Git LFS** —
run `git lfs install` once before cloning/pushing. See `.gitattributes` for tracked
extensions.

## License

Case study write-ups and documentation are licensed under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — reuse with attribution.
Client-identifying details are omitted or genericized; underlying full reports are
available on request.
