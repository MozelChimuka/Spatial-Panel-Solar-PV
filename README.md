# Spatial Panel Econometrics: Weather and Solar PV Yield Across Zambia (2020–2025)

**Author:** Mozel Chimuka | Economics & Mathematics, University of Zambia

**Type:** Self-directed methods practice using real weather data and *simulated* PV yield

**Full write-up:** [Zambia_Solar_Yield_Panel_Analysis.pdf](Zambia_Solar_Yield_Panel_Analysis.pdf)

## Summary
Solar irradiance (GHI) is the main driver of PV output, but panel efficiency also falls as temperature rises above the 25°C standard test condition. This project builds a balanced panel covering all ten Zambian provinces and tests whether a one-way fixed effects model can detect and quantify that thermal penalty.

> **Important:** Weather inputs are real; PV yield is **simulated**. Results validate a method. They are **not** engineering yield estimates for Zambian solar plants.

## Data
| Item | Detail |
|---|---|
| Panel | 10 provinces × 2,192 days = **21,920 observations** (balanced), 2020–2025 |
| Weather inputs | Daily GHI (kWh/m²/day) and ambient temperature (°C) from the Open-Meteo Historical Weather API (ECMWF ERA5 reanalysis) |
| Yield (simulated) | Baseline efficiency of 18%, a thermal derating of 0.4% per °C above 25°C (scaled by irradiance), and additive random noise |

## Method
One-way panel fixed effects (within estimator):

`Yield_it = α_i + β1·GHI_it + β2·Temperature_it + ε_it`

- `α_i` absorbs time-invariant province differences (altitude, baseline climate)
- Compared against a pooled OLS baseline
- Diagnostics: variance inflation factor and residual plots

## Results
| Variable | Coefficient |
|---|---|
| GHI | 0.18009*** |
| Temperature | −0.00079*** |

R² = 0.9388 | Adj. R² = 0.93878 | N = 21,920 | VIF = 1.40

- GHI is the dominant driver of simulated yield. Its coefficient (about 0.18) matches the baseline efficiency built into the simulation.
- Temperature has a small, statistically significant negative coefficient, consistent with the thermal derating built into the simulation.
- Fixed-effects and pooled OLS slopes are nearly identical.
- **Recovery check:** the simulated penalty is a 0.4% efficiency loss per °C above 25°C, scaled by irradiance. The model detects its sign and significance, but the linear temperature coefficient is not directly comparable in magnitude, because the true penalty is multiplicative and applies only above 25°C.

See the PDF for the full regression table, figures and discussion.

## Limitations
- **Simulated outcome.** Yield is generated from the same weather inputs, so the strong fit is partly by construction. Results should be calibrated against real inverter data before any engineering conclusions.
- **Specification.** A GHI × temperature interaction, with a threshold at 25°C, would match the simulated penalty more closely than a plain linear temperature term. This is a natural next step.
- **Ambient vs cell temperature.** The model uses ambient air temperature; real panel efficiency depends on cell temperature, which is typically higher.
- **Few clusters.** With only 10 provinces, cluster-robust standard errors can be unreliable. Wild-bootstrap or small-sample corrections would be more conservative.
- **Diagnostics.** The residual plots shown in the write-up are for the pooled OLS specification.
- **Omitted factors.** Cloud cover, humidity, wind, and dust or soiling losses are not modelled.

## Code
Code is not included in this repository. The write-up documents the data, specification and results.

## Data credit
Weather data: Open-Meteo Historical Weather API, based on ECMWF ERA5 reanalysis.
