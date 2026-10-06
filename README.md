# Spatial Panel Econometrics: Weather and Solar PV Yield Across Zambia (2020–2025)

**Author:** Mozel Chimuka | Economics & Mathematics, University of Zambia
**Type:** Self-directed methods practice using real weather data and *simulated* PV yield

## Summary
Solar irradiance (GHI) is the main driver of PV output, but panel efficiency also falls as cell temperature rises above the 25°C standard test condition. This project builds a balanced panel covering all ten Zambian provinces and tests whether a one-way fixed effects model can detect and quantify that thermal penalty.

> **Important:** Weather inputs are real; PV yield is **simulated**. Results validate a method. They are **not** engineering yield estimates for Zambian solar plants.

## Data
| Item | Detail |
|---|---|
| Panel | 10 provinces × 2,192 days = **21,920 observations** (balanced), 2020–2025 |
| Weather inputs | Daily GHI (kWh/m²/day) and ambient temperature (°C) from the Open-Meteo Historical Weather API (ECMWF ERA5 reanalysis) |
| Yield (simulated) | Fixed baseline panel efficiency, a linear thermal derating penalty above 25°C, and an additive random noise term |

## Method
One-way panel fixed effects (within estimator):

`Yield_it = α_i + β1·GHI_it + β2·Temperature_it + ε_it`

- `α_i` absorbs time-invariant province differences (altitude, baseline climate)
- Standard errors clustered at province level
- Diagnostics: VIF, residuals vs fitted, Q-Q, scale-location, leverage
- Compared against a pooled OLS baseline

## Results
| Variable | Coefficient | Clustered SE |
|---|---|---|
| GHI | 0.18009*** | 0.000224 |
| Temperature | −0.00079*** | 0.000171 |

R² = 0.9388 | Adj. R² = 0.93878 | F = 168,062 | N = 21,920 | VIF = 1.40

- GHI is the dominant driver of simulated yield.
- Temperature has a small, statistically significant negative coefficient, consistent with the thermal derating built into the simulation.
- Fixed-effects and pooled OLS slopes are nearly identical.
- **Recovery check:** the simulation used a derating of `[ADD TRUE VALUE]` per °C above 25°C; the model estimated `[ADD ESTIMATE]`. `[One sentence on how close they are.]`

## Limitations
- **Simulated outcome.** Yield is generated from the same weather inputs, so strong fit is partly by construction. Calibrate against real inverter data before drawing engineering conclusions.
- **Specification.** Derating applies only above 25°C and scales with irradiance, but the model uses a simple linear temperature term. Threshold or interaction terms (GHI × temperature) are a natural next step.
- **Few clusters.** Only 10 provinces are available for clustering, so cluster-robust standard errors may be too optimistic. Wild-bootstrap or small-sample corrections would be more conservative.
- **Omitted factors.** Cloud cover, humidity, wind, and dust/soiling are not modelled.

## Reproducing
1. Download daily GHI and temperature for the 10 provincial coordinates from Open-Meteo (2020-01-01 to 2025-12-31).
2. Run the simulation script, then the estimation script. `[Add file names and required packages]`
3. Outputs: regression table, Figure 1, diagnostic plots.

Full write-up: `Zambia_Solar_Yield_Panel_Analysis.pdf`

## Data credit
Weather data: Open-Meteo Historical Weather API, based on ECMWF ERA5 reanalysis.
