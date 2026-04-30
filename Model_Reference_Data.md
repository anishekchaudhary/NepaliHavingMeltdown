# Nepal Himalayan Glacier System Dynamics — Model Reference Data

**Model file:** `Nepal_SFD.mdl`
**Simulation period:** 1980–2100
**Time step:** 1 year
**Prepared:** 2026-04-30

---

## 1. All Initial Values (with Citations)

### 1.1 Stock Initial Values

| Stock | Initial Value | Unit | Citation / Justification |
|---|---|---|---|
| GreenHouseGas | 830 | Gt CO₂ | IPCC AR6 2021; ~338 ppm × 2.13 Gt/ppm |
| Glaciers Volume | 312 | km³ | ICIMOD HI-WISE 2023 |
| Snow Cover | 50 | km³ | Brun et al. 2017, HMA estimates |
| Glacial Lake | 0.5 | km³ | Realistic 1980 aggregate; Rounce et al. PNAS 2020 |
| Forest Cover | 64,000 | km² | Global Forest Watch 2024; Nepal = 6.4 M ha |
| Black Carbon | 120 | Gg | Equilibrium: BC Emission / BC Lifetime = 60 / 0.5; ICIMOD 2021 |

### 1.2 Parameter Values

| Parameter | Value | Unit | Citation / Justification |
|---|---|---|---|
| Initial GHG | 830 | Gt CO₂ | Same as GreenHouseGas stock at t=1980 |
| Initial Ice | 362 | km³ | 312 (glaciers) + 50 (snow) at t=1980 |
| Initial Black Carbon | 120 | Gg | BC Emission at 1980 / BC Lifetime Fraction = 60 / 0.5 |
| Base Temperature | 14 | °C | World Bank Nepal 2021 mean surface temperature |
| Permafrost Thaw Threshold | 1.5 | °C | Schuur et al. 2015 (above-baseline warming trigger) |
| Accumulation Rate | 0.55 | Dmnl | Stigter et al. 2021: Yala glacier accumulation-to-precipitation ratio |
| Surface Melt Rate | 0.25 | km³/yr/°C | Calibrated to HMA melt observations |
| Melt Sensitivity | 0.47 | km/yr/°C | Maurer et al. 2021 HMA calibration: 0.17 km³/yr/°C ÷ 0.363 km² |
| Glacier Area | 0.363 | km² | ICIMOD 2023: 3,902 km² total; 363 km² effective (scaled) |
| Base Precipitation | 1.5 | km/yr | Sherpa et al. 2020: Khumbu region 1,200–2,000 mm/yr |
| Snow Area | 0.8 | km² | Effective catchment area for snow accumulation |
| Lake Feed Fraction | 0.3 | Dmnl | Rounce et al. 2020; fraction of meltwater feeding glacial lakes |
| Drainage Fraction | 0.08 | 1/yr | PNAS 2020; annual outflow rate from glacial lakes |
| Undercutting Coefficient | 0.025 | 1/(yr·°C) | Calibrated to observed thermal undercutting rates |
| GLOF Threshold | 5 | km³ | Nepal priority lakes assessment; 47 high-risk lakes |
| Base Afforestation Rate | 0.014 | 1/yr | Global Forest Watch 2024 Nepal reforestation data |
| Base Deforestation Rate | 0.012 | 1/yr | Global Forest Watch 2024 Nepal land-use data |
| BC Lifetime Fraction | 0.5 | 1/yr | ~2-year atmospheric lifetime of black carbon; ICIMOD 2021 |
| BC Emission (1980) | 60 | Gg/yr | ICIMOD 2021 Nepal/HMA black carbon emission inventory |
| BC Emission Growth Rate | 1.2% | /yr | ICIMOD 2021 projected growth |
| Human Emission (1980) | 36 | Gt CO₂/yr | IPCC AR6; Global Carbon Project 2022 |
| Human Emission Growth Rate | 1.8% | /yr | IPCC AR6; Global Carbon Project 2022 historical trend |

---

## 2. All Equations (with Assumptions)

### 2.1 GHG and Emissions Subsystem

| Variable | Equation | Assumption | Source |
|---|---|---|---|
| GreenHouseGas | `INTEG(Emission - Removal, 830)` | Stock begins at 1980 CO₂ equivalent level | IPCC AR6 |
| Emission | `Human Emission + Permafrost Emission` | Only two sources of net GHG emission modeled | Simplified carbon cycle |
| Human Emission | `36 * EXP(0.018 * (Time - 1980))` | Exponential growth at 1.8%/yr from 1980 baseline | IPCC AR6; Global Carbon Project 2022 |
| Permafrost Emission | `IF THEN ELSE(Delta Temperature > 1.5, 0.020*(Delta Temperature-1.5), 0)` | Emissions only trigger above 1.5°C warming threshold; linear above threshold | Schuur et al. 2015 |
| Removal | `Ocean Sink + Terrestrial Sink` | Total sink is sum of ocean and land uptake | Global carbon cycle literature |
| Ocean Sink | `10.6 * LN(MAX(GreenHouseGas/Initial GHG, 1) + 1)` | Logarithmic uptake; 10.6 Gt/yr scaling; already absorbing ~7.35 Gt at 1980 (realistic) | Calibrated to Global Carbon Project 2022 |
| Terrestrial Sink | `3.1 + 3.9e-08 * Forest Cover` | Global base sink = 3.1 Gt/yr; Nepal forest adds ~0.0025 Gt/yr (negligible at global scale) | IPCC AR6; Nepal-specific coefficient |

### 2.2 Temperature Subsystem

| Variable | Equation | Assumption | Source |
|---|---|---|---|
| Temperature | `Base Temperature + Delta Temperature` | Linear superposition of background and anomaly | Standard climate model structure |
| Delta Temperature | `GHG Temperature Effect + Albedo Temperature Effect` | Temperature anomaly from two physical drivers | Simplified energy balance |
| GHG Temperature Effect | `3 * LN(MAX(GreenHouseGas/Initial GHG, 1))` | ECS ~3°C per doubling (simplified; note: missing /LN(2) — see Issue 1 below) | Myhre 1998; IPCC AR6 |
| Albedo Temperature Effect | `1.5 * (0.62 - Surface Albedo)` | 1.5°C sensitivity per albedo unit deviation from reference 0.62 | Simplified energy balance |

### 2.3 Albedo and Ice Cover Subsystem

| Variable | Equation | Assumption | Source |
|---|---|---|---|
| Surface Albedo | `MAX(0.15, 0.25 + 0.37*Ice Fraction - BC Albedo Effect)` | Floor of 0.15 (bare rock); clean glacier albedo ~0.62 at full ice | Bolch 2012; IPCC cryosphere |
| BC Albedo Effect | `0.038 * MIN(1, Black Carbon/Initial Black Carbon)` | Max BC albedo reduction = 0.038; saturates at initial BC level | ICIMOD 2021 |
| Ice Fraction | `MIN(1, MAX(0, Total Ice/Initial Ice))` | Bounded [0,1]; normalized to 1980 total ice volume | Physical constraint |
| Total Ice | `Glaciers Volume + Snow Cover` | Combined ice mass for albedo calculation | Physical definition |

### 2.4 Glacier and Snow Subsystem

| Variable | Equation | Assumption | Source |
|---|---|---|---|
| Glaciers Volume | `INTEG(Glacial Deposit - Glacier Melt Rate, 312)` | Net balance of accumulation and melt | ICIMOD HI-WISE 2023 |
| Glacial Deposit | `MAX(0, Accumulation Rate*Annual Snowfall - Surface Melt Rate*Delta Temperature)` | Net snowpack transfer to glaciers; clamped at zero | Stigter et al. 2021 |
| Annual Snowfall | `Snow Probability * Base Precipitation * Glacier Area` | Snowfall scales with precipitation and temperature-dependent snow probability | Sherpa et al. 2020 |
| Glacier Melt Rate | `MAX(0, (Melt Sensitivity + BC Melt Enhancement)*Delta Temperature*Glacier Area)` | Temperature-driven melt enhanced by black carbon deposition | Maurer et al. 2021 |
| BC Melt Enhancement | `0.39 * Melt Sensitivity * MIN(1, Black Carbon/Initial Black Carbon)` | BC darkening increases melt by up to 39% of base sensitivity | ICIMOD 2021 |
| Snow Cover | `INTEG(Snowfall Precipitation - Glacial Deposit, 50)` | Snow accumulates via precipitation, drains into glaciers | Brun et al. 2017 |
| Snowfall Precipitation | `Snow Probability * Base Precipitation * Snow Area` | Same snow probability logic applied to larger catchment area | Sherpa et al. 2020 |
| Snow Probability | `MAX(0, MIN(1, 1/(1+EXP(1.2*(Temperature-3)))))` | Logistic function: probability of snow vs rain based on temperature; threshold ~3°C | Standard threshold approach |

### 2.5 Glacial Lake and GLOF Subsystem

| Variable | Equation | Assumption | Source |
|---|---|---|---|
| Glacial Lake | `INTEG(Meltwater Inflow - Lake Drainage, 0.5)` | Lake volume accumulates from meltwater and thermal undercutting | Rounce et al. PNAS 2020 |
| Meltwater Inflow | `Lake Feed Fraction * Glacier Melt Rate + Thermal Undercutting` | Fraction of meltwater feeds lakes; undercutting adds to inflow | Rounce et al. 2020 |
| Lake Drainage | `Drainage Fraction * Glacial Lake` | First-order outflow from lake (natural drainage/seepage) | PNAS 2020 |
| Thermal Undercutting | `Undercutting Coefficient * Glacial Lake * MAX(0, Delta Temperature)` | Warm-water erosion of ice dams scales with lake volume and warming | Observed HMA dynamics |
| GLOF Risk Index | `MIN(1, Glacial Lake/GLOF Threshold)` | Normalized risk index; reaches 1.0 when lake volume hits threshold | Nepal GLOF assessment |

### 2.6 Forest Cover Subsystem

| Variable | Equation | Assumption | Source |
|---|---|---|---|
| Forest Cover | `INTEG(Afforestation - Deforestation, 64000)` | Net forest change from planting and clearing | Global Forest Watch 2024 |
| Afforestation | `Base Afforestation Rate * Forest Cover` | Proportional to existing forest (natural regeneration + planting) | GFW 2024 |
| Deforestation | `(Base Deforestation Rate + Flood Pressure) * Forest Cover` | Flood events from GLOF increase pressure on forest resources | GFW 2024; model assumption |
| Flood Pressure | `0.002 * GLOF Risk Index` | GLOF risk adds a small additional deforestation pressure (0–0.2%/yr) | Model assumption; calibrated |

### 2.7 Black Carbon Subsystem

| Variable | Equation | Assumption | Source |
|---|---|---|---|
| Black Carbon | `INTEG(BC Emission - BC Decay, 120)` | Atmospheric BC stock in equilibrium at 1980 | ICIMOD 2021 |
| BC Emission | `60 * EXP(0.012 * (Time - 1980))` | Exponential growth at 1.2%/yr from 60 Gg/yr in 1980 | ICIMOD 2021 |
| BC Decay | `BC Lifetime Fraction * Black Carbon` | First-order removal; ~2-year atmospheric lifetime | ICIMOD 2021 |

### 2.8 Control Parameters

| Variable | Equation | Unit |
|---|---|---|
| INITIAL TIME | `1980` | Year |
| FINAL TIME | `2100` | Year |
| TIME STEP | `1` | Year |
| SAVEPER | `TIME STEP` | Year |
| Base Temperature | `14` | °C |
| Permafrost Threshold | `1.5` | °C |
| Accumulation Rate | `0.55` | Dmnl |
| Surface Melt Rate | `0.25` | km³/yr/°C |
| Melt Sensitivity | `0.47` | km/yr/°C |
| Glacier Area | `0.363` | km² |
| Base Precipitation | `1.5` | km/yr |
| Snow Area | `0.8` | km² |
| Lake Feed Fraction | `0.3` | Dmnl |
| Drainage Fraction | `0.08` | 1/yr |
| Undercutting Coefficient | `0.025` | 1/(yr·°C) |
| GLOF Threshold | `5` | km³ |
| Base Afforestation Rate | `0.014` | 1/yr |
| Base Deforestation Rate | `0.012` | 1/yr |
| BC Lifetime Fraction | `0.5` | 1/yr |
| Initial GHG | `830` | Gt CO₂ |
| Initial Ice | `362` | km³ |
| Initial Black Carbon | `120` | Gg |

---

## 3. Model Structural Issues Found

### Issue 1: GHG Temperature Effect — Missing /LN(2) Divisor (MODERATE SEVERITY)

**Current equation:** `GHG Temperature Effect = 3 * LN(MAX(GreenHouseGas/Initial GHG, 1))`

**Correct per Myhre 1998:** `3 * LN(MAX(GreenHouseGas/Initial GHG, 1)) / LN(2)`

**Impact:**
- The Equilibrium Climate Sensitivity (ECS) parameter 3°C is defined as warming *per doubling* of CO₂.
- A true doubling means GHG/GHG₀ = 2, so LN(2/1) = 0.693. Without /LN(2): 3 × 0.693 = **2.08°C** per doubling.
- With /LN(2): 3 × 0.693 / 0.693 = **3.00°C** per doubling (correct).
- The current formula underestimates warming by approximately **30%**.

**Recommendation:** Fix to `3 * LN(MAX(GreenHouseGas/Initial GHG, 1)) / LN(2)`

---

### Issue 2: GLOF Threshold Value Inconsistency Between Files (LOW SEVERITY)

**In Equations_Reference.md:** GLOF Threshold = 50 km³ (stated as "calibrated to 47 Nepal lakes × ~1 km³")

**In Nepal_SFD.mdl:** `GLOF Threshold = 5` km³

**Analysis:**
- With initial Glacial Lake = 0.5 km³ and threshold = 5 km³: GLOF Risk Index = 0.1 at t=0 (10% risk at start — physically reasonable).
- With threshold = 50 km³: GLOF Risk Index = 0.01 at t=0 (1% — unrealistically low given Nepal's known lake hazards).
- 5 km³ is more defensible for a model calibrated to Nepal's 47 highest-priority glacial lakes.

**Recommendation:** Keep GLOF Threshold = 5 km³ in the model. Update Equations_Reference.md to reflect this value and correct the cited justification.

---

### Issue 3: Ocean Sink Formula — Not a Bug (INFORMATIONAL)

**Model equation:** `10.6 * LN(MAX(GreenHouseGas/Initial GHG, 1) + 1)`

**At t=1980:** MAX(830/830, 1) + 1 = 2 → Sink = 10.6 × LN(2) ≈ **7.35 Gt/yr**

**Assessment:** This is correct behavior, not a bug. The ocean was already absorbing significant CO₂ in 1980. The Global Carbon Project records ~10 Gt/yr ocean uptake by 2020, so 7.35 Gt/yr in 1980 is physically reasonable and consistent with the literature.

**No action required.**

---

### Issue 4: Terrestrial Sink — Nepal Coefficient Makes Forest Subsystem Globally Negligible (INFORMATIONAL)

**Coefficient:** `3.9e-08 * Forest Cover` → at 64,000 km²: **0.0025 Gt CO₂/yr**

**Global base sink:** 3.1 Gt CO₂/yr

**Assessment:** Nepal's forests (~0.0025 Gt/yr) contribute ~0.08% of the global terrestrial sink. Changes in Forest Cover have essentially no effect on GreenHouseGas dynamics at the global scale. This means the "B2: Forest Carbon Sink" balancing loop in the model is structurally correct but functionally negligible.

**Scientifically:** This is accurate. Nepal's forests are locally important (biodiversity, erosion control, water regulation) but cannot meaningfully alter global CO₂ trajectories.

**Recommendation:** Accept as-is. Document explicitly in presentations that the Forest Cover subsystem's primary model function is the GLOF→Deforestation linkage and local ecological dynamics, not global GHG regulation.

---

### Issue 5: Snow Cover Drainage to Zero — Physically Correct (INFORMATIONAL)

**Mechanism:** Snow Cover drains into Glaciers Volume via Glacial Deposit. If Snow Probability → 0 (high temperature), Snowfall Precipitation → 0 and Snow Cover can drain to zero.

**MAX(0,...) clamp:** Glacial Deposit is clamped at zero, preventing negative transfer.

**Assessment:** This is physically correct behavior. At sufficiently high temperatures, snow cover disappears — the model captures this appropriately. No structural fix needed.

---

### Issue 6: Black Carbon Equilibrium — Confirmed Correct (INFORMATIONAL)

**At t=1980:**
- BC Emission = 60 Gg/yr
- BC Lifetime = 1 / 0.5 = 2 years
- Equilibrium stock = 60 / 0.5 = **120 Gg**
- Initial Black Carbon = **120 Gg**

**Assessment:** The stock correctly begins at its equilibrium value. No initialization error exists.

---

### Issue 7: Surface Albedo Lower Bound — Correctly Handled (INFORMATIONAL)

**Equation:** `MAX(0.15, 0.25 + 0.37*Ice Fraction - BC Albedo Effect)`

**Check:**
- At Ice Fraction = 1, BC = 0: Albedo = 0.25 + 0.37 = **0.62** (matches Bolch 2012 clean glacier value)
- At Ice Fraction = 0, high BC: Could go below 0.15, but clamped at **0.15** (bare dark rock — physically realistic)

**Assessment:** Bounds are correctly implemented. No issue.

---

## 4. Recommendations for Policy Testing

### 4.1 Key Policy Levers

| Lever | Model Parameter | Current Value | Testable Range |
|---|---|---|---|
| Black carbon emission control | BC Emission growth rate | 1.2%/yr | −1.2%/yr (decline) to 0%/yr (flat) |
| CO₂ mitigation policy | Human Emission growth rate | 1.8%/yr | −2%/yr to +1.8%/yr |
| Forest protection | Base Deforestation Rate | 0.012 1/yr | 0.006–0.020 1/yr |
| Reforestation programs | Base Afforestation Rate | 0.014 1/yr | 0.010–0.025 1/yr |
| GLOF early warning | GLOF Threshold | 5 km³ | Sensitivity: 3–10 km³ |

### 4.2 Recommended Scenarios

**Scenario 1: Business as Usual (BAU)**
- All parameters at current calibrated values.
- Human Emission: +1.8%/yr; BC Emission: +1.2%/yr.
- Baseline for comparison.

**Scenario 2: Black Carbon Reduction (Clean Energy)**
- Reduce BC Emission growth to 0% (flat from 1980 levels) or declining.
- Represents cookstove replacement, diesel vehicle regulation, and industrial controls.
- Expected effect: Reduced BC Albedo Effect → higher Surface Albedo → slower warming; reduced BC Melt Enhancement → slower glacier melt.
- Key output variables to track: Surface Albedo, Glacier Melt Rate, Glaciers Volume, GLOF Risk Index.

**Scenario 3: Paris Agreement CO₂ Trajectories**
- Test three sub-scenarios:
  - *1.5°C pathway:* Human Emission peaks 2025, declines to net zero by 2050 (growth → −3%/yr).
  - *2°C pathway:* Human Emission peaks 2030, declines to net zero by 2070.
  - *NDC pathway:* Human Emission grows at reduced +0.5%/yr through 2040, then plateaus.
- Key output variables: Delta Temperature, GreenHouseGas, Permafrost Emission, Glaciers Volume.

**Scenario 4: Forest Protection and Restoration**
- Increase Base Afforestation Rate from 0.014 to 0.022 1/yr (doubles Nepal planting ambition).
- Decrease Base Deforestation Rate from 0.012 to 0.006 1/yr (strict enforcement).
- Expected effect: Larger Forest Cover → lower Deforestation via reduced Flood Pressure feedback; marginal Terrestrial Sink increase (small at global scale).
- Key output variables: Forest Cover trajectory, Deforestation rate, GLOF Risk Index (indirect via Flood Pressure).

**Scenario 5: GLOF Early Warning System (Threshold Sensitivity)**
- Test GLOF Threshold at 3, 5, and 8 km³ to assess sensitivity of risk triggers.
- Represents different early warning trigger levels for evacuation and infrastructure protection.
- Key output: GLOF Risk Index time series under BAU warming.

**Scenario 6: Combined Policy Package**
- Scenario 2 + Scenario 3 (1.5°C) + Scenario 4 combined.
- Tests whether co-benefits of clean energy, CO₂ mitigation, and forest restoration can stabilize Himalayan glacier systems.
- Expected: Significant reduction in glacier melt rate, GLOF risk, and temperature anomaly relative to BAU.

### 4.3 Key Feedback Loops to Monitor

| Loop | Type | Variables | Policy Relevance |
|---|---|---|---|
| R1: GHG–Warming–Permafrost | Reinforcing | GHG → Temperature → Permafrost Emission → GHG | Tipping point risk; critical if Delta T exceeds 1.5°C |
| R2: Ice–Albedo–Warming | Reinforcing | Ice Fraction → Surface Albedo → Temperature → Melt → Ice | Accelerating glacier loss |
| B1: Ocean Carbon Sink | Balancing | GHG → Ocean Sink → GHG | Self-limiting CO₂ growth |
| B2: Forest Carbon Sink | Balancing | Forest Cover → Terrestrial Sink → GHG | Negligible at Nepal scale (see Issue 4) |
| R3: BC–Melt Enhancement | Reinforcing | BC → BC Melt Enhancement → Glacier Melt Rate | Black carbon amplifier; policy-amenable |
| R4: GLOF–Deforestation | Reinforcing | GLOF Risk → Flood Pressure → Deforestation → Forest Cover | Cascading risk loop |

---

*Document prepared from Nepal_SFD.mdl equations and calibration data. Cross-referenced with Equations_Reference.md and literature sources as cited.*
