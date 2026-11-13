# Validation results

Tests are defined in docs/04-Science-Validation.md §7. Fill a row only with real results.

| ID | Test | Result | Pass? | Evidence |
|---|---|---|---|---|
| V1 | Barnard's Star proper motion | μ = 10.56 ± 0.33″/yr, PA 355.1° vs Gaia 10.39″/yr, 355.6° (1.6 %, 0.4°) | ✅ PASS (with Gaia epoch) | Shivaansh, Nov 12: Gaia DR3 2016.0 position + brightest pixel in 4 real frames (D3/D6, MJD 60902–61271), `motion.fit_proper_motion`. Re-run by Rishabh: same result. SPHEREx frames alone give 13.9 ± 3.0″/yr at 17.7° (whole-pixel positions on a saturated core), so the Gaia epoch carries this; sub-pixel centroids needed |
| V2 | ε Indi B proper motion | not run yet | | First attempt (Nov 12) used the baked `target_xy`, which is the Gaia-predicted position, not a measurement, so it was dropped. Needs positions measured in the SPHEREx frames |
| V3 | WISE 0855 detection & motion | not run yet | | Finding (Nov 12): a Gaia DR3 star (G 16.3) sits 5–9″ from WISE 0855 during the SPHEREx year, inside our 3-pixel measuring circle. Before the blend check, its light gave a false 1 mJy “spectrum”. Now all 256 points are rejected (blend, too faint or flagged), so the spectrum says “not measured” rather than showing the neighbour's light. Motion still to test |
| V4 | Our photometry vs IRSA tool | Median flux ratio = 0.966 across 3 unsaturated stars (star medians: 0.988, 0.941, 0.988; range 0.874–1.089) | ✅ PASS | Tirth, Nov 13: 3 isolated unsaturated 2MASS AAA stars in real Barnard cutouts (D2/D3, 1.14–1.66 µm), `photometry.measure` vs IRSA `fp_psc` TAP reference fluxes (Cohen et al. 2003 zero points); see table below |
| V5 | Brown-dwarf typing | ε Indi B typed T3 (unresolved T1+T6 pair); distance 2.55–3.69 pc vs 3.64 pc from Gaia parallax | ✅ PASS | Rishabh, Nov 12: 304 of 445 real SPHEREx points (QR2+QR3, D1–D6), `typing.spectrum_opinion`, SpeX standards + Dupuy & Liu 2012; baked in `web/public/data/targets/epsindib/spectrum.json` |
| V6 | Known asteroids | cut | — | D3.4 known asteroids cut on Nov 12 (docs/DECISIONS.md) |
| V7 | Thermometer | not run yet | | D4.2 back in scope (Nov 12, DECISIONS). `thermal.py` built and model-tested (geometry, recovery of a model distance); V7 needs real D3/D6 pairs of ≥ 20 known asteroids from D3.4 |
| V8 | Variables | not run yet | | |
| V9 | False-alarm control | not run yet | | |
| V10 | Reproducibility | not run yet | | |

## Detail

### V4 — Photometry vs IRSA reference fluxes (3 unsaturated stars)

Measured with `planetes.photometry.measure` (3.0 px radius aperture, 6–9 px sigma-clipped sky annulus, PSF-zone aperture correction, and local WCS Jacobian solid angle) on real SPHEREx QR2/QR3 cutouts in the Barnard's Star field, compared against IRSA's 2MASS Point Source Catalog (`fp_psc`, `ph_qual = AAA`, Cohen et al. 2003 zero points: $J_{1.235\,\mu\text{m}} = 1594\text{ Jy}$, $H_{1.662\,\mu\text{m}} = 1024\text{ Jy}$):

| Star (2MASS ID) | RA, Dec (deg) | 2MASS J / H (mag) | Band ($\lambda_{\text{SPHEREx}}$) | Planetes Flux ($\mu\text{Jy}$) | IRSA 2MASS Flux ($\mu\text{Jy}$) | Ratio (Planetes / IRSA) | Star Median Ratio |
|---|---|---|---|---:|---:|---:|---:|
| `2MASS J17574401+0444175` | `269.433402, +4.738199` | 12.68 / 12.11 | J (`1.140 µm`) | 12,252.5 | 13,480.0 | 0.909 | **0.988** |
| `2MASS J17574401+0444175` | `269.433402, +4.738199` | 12.68 / 12.11 | J (`1.210 µm`) | 12,574.9 | 13,480.0 | 0.933 | — |
| `2MASS J17574401+0444175` | `269.433402, +4.738199` | 12.68 / 12.11 | H (`1.629 µm`) | 14,515.6 | 14,692.6 | 0.988 | — |
| `2MASS J17574401+0444175` | `269.433402, +4.738199` | 12.68 / 12.11 | H (`1.645 µm`) | 14,825.9 | 14,692.6 | 1.009 | — |
| `2MASS J17574401+0444175` | `269.433402, +4.738199` | 12.68 / 12.11 | H (`1.658 µm`) | 16,003.4 | 14,692.6 | 1.089 | — |
| `2MASS J17574602+0444029` | `269.441779, +4.734149` | 13.85 / 13.35 | J (`1.140 µm`) | 4,319.9 | 4,588.7 | 0.941 | **0.941** |
| `2MASS J17574602+0444029` | `269.441779, +4.734149` | 13.85 / 13.35 | J (`1.210 µm`) | 4,430.9 | 4,588.7 | 0.966 | — |
| `2MASS J17574602+0444029` | `269.441779, +4.734149` | 13.85 / 13.35 | H (`1.658 µm`) | 4,236.5 | 4,667.7 | 0.908 | — |
| `2MASS J17574088+0443538` | `269.420352, +4.731636` | 11.13 / 10.58 | J (`1.140 µm`) | 48,965.1 | 56,038.7 | 0.874 | **0.988** |
| `2MASS J17574088+0443538` | `269.420352, +4.731636` | 11.13 / 10.58 | J (`1.210 µm`) | 52,647.1 | 56,038.7 | 0.939 | — |
| `2MASS J17574088+0443538` | `269.420352, +4.731636` | 11.13 / 10.58 | H (`1.629 µm`) | 62,121.8 | 59,910.1 | 1.037 | — |
| `2MASS J17574088+0443538` | `269.420352, +4.731636` | 11.13 / 10.58 | H (`1.658 µm`) | 62,556.6 | 59,910.1 | 1.044 | — |

- **Overall median across all 12 measurements:** `0.966` (star-by-star medians: `0.988`, `0.941`, `0.988` — all within the `0.9–1.1` pass criterion).
