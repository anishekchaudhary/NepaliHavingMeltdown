# Nepal Glacier SFD — Complete Context for New Chat

## What This Project Is

CS752 System Dynamics course project at IIT Bombay. We are building a **Stock-and-Flow Diagram (SFD)** in Vensim to model how greenhouse gas emissions drive Himalayan glacier melt in Nepal from 1980–2080. The model is Nepal-specific and calibrated to real field data.

**Main file:** `Nepal_SFD.mdl` (open in Vensim PLE or Vensim DSS)
**Secondary file:** `Nepal_CLD.mdl` (Causal Loop Diagram — conceptual overview only, no equations)
**Documentation:** `Model_Documentation.html`, `Equations_Reference.md`, `Model_Reference_Data.md`

---

## Vensim MDL File Format — Critical Rules

### Equation Section
Every variable needs a real equation. Format:
```
VariableName=
    equation here
    ~   Unit
    ~       |
```

Stock (uses INTEG):
```
GreenHouseGas= INTEG (
    Emission - Removal,
        830)
    ~   Gt CO2
    ~       |
```

### Control Section (always at end of equations)
```
FINAL TIME  = 2080
    ~   Year
    ~   The final time for the simulation.
    |

INITIAL TIME  = 1980
    ~   Year
    ~   The initial time for the simulation.
    |

SAVEPER  =
        TIME STEP
    ~   Year [0,?]
    ~   The frequency with which output is stored.
    |

TIME STEP  = 0.125
    ~   Year [0,?]
    ~   The time step for the simulation.
    |
```

### Sketch Section (after `V300  Do not put anything below this section`)
Arrow format (CRITICAL — must follow exactly):
```
1,ArrowID,FromNodeID,ToNodeID,1,0,polarity,0,1,192,0,COLOR,|||0-0-0,1|(bendX,bendY)|
```
- Field 5 = `1` (curved) — **must be 1 or arrow is invisible**
- Field 9 = `1` — **must be 1 for color to show**
- Bend coordinates must be real midpoint, NOT `(0,0)`
- Polarity: `43` = positive (+), `45` = negative (−)

Arrow colors by subsystem:
- GHG: `255-128-0` (orange)
- Temperature/Albedo: `192-0-0` (dark red)
- Glaciers/Snow: `0-0-200` (blue)
- Glacial Lake/GLOF: `0-160-0` (green)
- Forest: `0-110-0` (dark green)
- Black Carbon: `128-0-128` (purple)

Node formats:
```
10,ID,Name,X,Y,W,H,8,3,...   ← auxiliary variable
10,ID,Name,X,Y,W,H,3,3,...   ← stock (box shape)
12,ID,48,X,Y,10,8,0,3,...    ← cloud (source/sink)
11,ID,0,X,Y,6,8,34,3,...     ← valve (flow connector)
10,ID,Name,X,Y,W,H,40,3,...  ← flow label
```

Flow arrow types: `100` = valve→cloud, `4` = valve→stock (these replace the normal `1`)

**NEVER define `Time`** — it is a reserved Vensim built-in.
**NEVER use `A FUNCTION OF(...)`** in the SFD — that is CLD-only placeholder syntax.

---

## The Model — 6 Stocks

### Simulation Setup
- INITIAL TIME = 1980, FINAL TIME = 2080, TIME STEP = 0.125 Year

### Stock 1: GreenHouseGas (830 Gt CO₂)
Global atmospheric CO₂. Driven up by human + permafrost emissions; pulled down by ocean + terrestrial sinks.

```
GreenHouseGas= INTEG(Emission - Removal, 830)    ~ Gt CO2

Emission= Human Emission + Permafrost Emission   ~ Gt CO2/Year
Removal= Ocean Sink + Terrestrial Sink            ~ Gt CO2/Year

Human Emission= 36 * EXP(0.018 * (Time - 1980))  ~ Gt CO2/Year
  [36 Gt CO2/yr in 1980, growing 1.8%/yr — IPCC AR6, Global Carbon Project 2022]

Permafrost Emission= IF THEN ELSE(Delta Temperature > Permafrost Thaw Threshold,
    0.020 * (Delta Temperature - Permafrost Thaw Threshold), 0)   ~ Gt CO2/Year
  [triggers above 1.5°C warming; 0.020 Gt/°C = Schuur 2015 global rate × 2.2% Nepal fraction]

Ocean Sink= 10.6 * LN(MAX(GreenHouseGas / Initial GHG, 1) + 1)   ~ Gt CO2/Year
  [logarithmic saturation; ~7.35 Gt/yr at 1980 — Friedlingstein et al. 2022]

Terrestrial Sink= 3.1 + 3.9e-08 * Forest Cover   ~ Gt CO2/Year
  [global baseline 3.1 Gt/yr + Nepal contribution 0.0025 Gt/yr — PMC 2024]

Permafrost Thaw Threshold= 1.5   ~ dmnl   [Schuur et al. 2015]
Initial GHG= 830   ~ Gt CO2
```

### Stock 2: Glaciers Volume (312 km³)
Nepal's permanent glacier ice. Gains from compacted snowfall; loses to temperature + BC-driven melt.

```
Glaciers Volume= INTEG(Glacial Deposit - Glacier Melt Rate, 312)   ~ km*km*km

Glacial Deposit= MAX(0, Accumulation Rate * Annual Snowfall - Surface Melt Rate * Delta Temperature)
  ~ km*km*km/Year   [Stigter et al. 2021: 55% accumulation fraction]

Annual Snowfall= Snow Probability * Base Precipitation * Glacier Area   ~ km*km*km/Year

Glacier Melt Rate= MAX(0, (Melt Sensitivity + BC Melt Enhancement) * Delta Temperature * Glacier Area)
  ~ km*km*km/Year   [Maurer et al. 2021; 0.47 km/yr/°C calibrated]

BC Melt Enhancement= 0.39 * Melt Sensitivity * MIN(1, Black Carbon / Initial Black Carbon)   ~ km/Year
  [ICIMOD 2021: BC accounts for 39% of pre-monsoon melt]

Accumulation Rate= 0.55   ~ dmnl   [Stigter et al. 2021]
Surface Melt Rate= 0.25   ~ km*km*km/Year
Melt Sensitivity= 0.47    ~ km/Year   [Maurer et al. 2021: 0.17 km³/yr/°C ÷ 0.363 km²]
Glacier Area= 0.363        ~ km*km   [ICIMOD 2023: 3,902 km² total; 363 km² effective]
```

### Stock 3: Snow Cover (50 km³)
Seasonal snowpack that feeds glaciers. Gains from snowfall; loses to Glacial Deposit (becomes glacier ice).

```
Snow Cover= INTEG(Snowfall Precipitation - Glacial Deposit, 50)   ~ km*km*km

Snowfall Precipitation= Snow Probability * Base Precipitation * Snow Area   ~ km*km*km/Year

Snow Probability= MAX(0, MIN(1, 1 / (1 + EXP(1.2 * (Temperature - 3)))))   ~ dmnl
  [sigmoid: 50% snow at 3°C — Sherpa et al. 2020]

Base Precipitation= 1.5   ~ km/Year   [Sherpa et al. 2020: Khumbu 1200–2000 mm/yr]
Snow Area= 0.8             ~ km*km
```

### Stock 4: Glacial Lake (0.5 km³)
Proglacial lake aggregate. Fills with meltwater + thermal undercutting; drains naturally.

```
Glacial Lake= INTEG(Meltwater Inflow - Lake Drainage, 0.5)   ~ km*km*km

Meltwater Inflow= Lake Feed Fraction * Glacier Melt Rate + Thermal Undercutting   ~ km*km*km/Year
Lake Drainage= Drainage Fraction * Glacial Lake   ~ km*km*km/Year

Thermal Undercutting= Undercutting Coefficient * Glacial Lake * MAX(0, Delta Temperature)
  ~ km*km*km/Year   [warm water undercuts glacier terminus → calving → R2 reinforcing loop]

GLOF Risk Index= MIN(1, Glacial Lake / GLOF Threshold)   ~ dmnl

Lake Feed Fraction= 0.3           [Rounce et al. PNAS 2020]
Drainage Fraction= 0.08           [PNAS 2020]
Undercutting Coefficient= 0.025
GLOF Threshold= 5                 ~ km*km*km
```

### Stock 5: Forest Cover (64,000 km²)
Nepal's forested area. Grows via afforestation; declines via deforestation amplified by GLOF flood pressure.

```
Forest Cover= INTEG(Afforestation - Deforestation, 64000)   ~ km*km

Afforestation= Base Afforestation Rate * Forest Cover    ~ km*km/Year
Deforestation= (Base Deforestation Rate + Flood Pressure) * Forest Cover   ~ km*km/Year

Flood Pressure= 0.002 * GLOF Risk Index   ~ 1/Year

Base Afforestation Rate= 0.014   ~ 1/Year   [GFW 2024]
Base Deforestation Rate= 0.012   ~ 1/Year   [GFW 2024]
```

### Stock 6: Black Carbon (120 Gg)
Atmospheric BC from South Asian combustion. Deposits on glaciers → reduces albedo + enhances melt.

```
Black Carbon= INTEG(BC Emission - BC Decay, 120)   ~ Gg

BC Emission= 60 * EXP(0.012 * (Time - 1980))   ~ Gg/Year   [ICIMOD 2021]
BC Decay= BC Lifetime Fraction * Black Carbon    ~ Gg/Year

BC Albedo Effect= 0.038 * MIN(1, Black Carbon / Initial Black Carbon)   ~ dmnl
  [ICIMOD 2021: max 3.8% albedo reduction]

BC Lifetime Fraction= 0.5   ~ 1/Year   [~2yr atmospheric lifetime]
Initial Black Carbon= 120   ~ Gg
```

---

## Temperature & Albedo Auxiliaries

```
Temperature= Base Temperature + Delta Temperature   ~ Celsius
Delta Temperature= GHG Temperature Effect + Albedo Temperature Effect   ~ dmnl

GHG Temperature Effect= 3 * LN(MAX(GreenHouseGas / Initial GHG, 1)) / LN(2)   ~ dmnl
  [Myhre 1998: 3°C per CO₂ doubling; /LN(2) is required for per-doubling calibration]

Albedo Temperature Effect= 1.5 * (0.62 - Surface Albedo)   ~ dmnl
  [Bolch 2012: 1.5°C sensitivity; 0.62 = clean glacier reference albedo]

Surface Albedo= MAX(0.15, 0.25 + 0.37 * Ice Fraction - BC Albedo Effect)   ~ dmnl
  [bare rock floor 0.15; glacier max 0.62; BC reduces albedo — Brun 2017]

Ice Fraction= MIN(1, MAX(0, Total Ice / Initial Ice))   ~ dmnl
Total Ice= Glaciers Volume + Snow Cover   ~ km*km*km
Ice Fraction= MIN(1, MAX(0, Total Ice / Initial Ice))   ~ dmnl

Base Temperature= 14   ~ Celsius   [World Bank Nepal 2021]
Initial Ice= 362       ~ km*km*km  [312 glaciers + 50 snow]
```

---

## Feedback Loops

| ID | Type | Path |
|----|------|------|
| **R1** Ice-Albedo | Reinforcing | GHG↑ → ΔT↑ → Glacier Melt↑ → Total Ice↓ → Surface Albedo↓ → Albedo Temp Effect↑ → ΔT↑ |
| **R2** GLOF Undercutting | Reinforcing | ΔT↑ → Melt↑ → Glacial Lake↑ → Thermal Undercutting↑ → Meltwater Inflow↑ → Glacial Lake↑ |
| **R3** Permafrost Trap | Reinforcing | ΔT > 1.5°C → Permafrost Emission↑ → GHG↑ → ΔT↑ |
| **R4** BC-Melt | Reinforcing | BC↑ → BC Albedo Effect↑ → Surface Albedo↓ → ΔT↑ → Glacier Melt↑ |
| **B1** Ocean Sink | Balancing | GHG↑ → Ocean Sink↑ (log saturation) → Removal↑ → GHG↓ |
| **B2** Forest Carbon | Balancing | Forest Cover → Terrestrial Sink↑ → Removal↑ → GHG↓ *(negligible at Nepal scale)* |
| **B3** GLOF-Deforestation | Weakens B2 | Glacial Lake↑ → GLOF Risk↑ → Flood Pressure↑ → Deforestation↑ → Forest Cover↓ |

---

## Known Issues & Decisions

1. **GHG Temperature Effect** — the `/LN(2)` divisor **was added** in the latest version. Without it, warming is underestimated by ~30%.

2. **GLOF Threshold = 5 km³** (not 50 — the old Equations_Reference.md had a typo). The model file is correct.

3. **Terrestrial Sink is globally negligible** — Nepal forests add only 0.0025 Gt/yr to global sink of 3.1 Gt/yr. This is scientifically correct; the forest subsystem mainly demonstrates the GLOF→deforestation cascade, not global carbon regulation.

4. **Initial Glacial Lake = 0.5 km³** — started at equilibrium (drainage = inflow at ΔT=0). Old value of 10 km³ caused the lake to drain immediately at t=0.

5. **Initial Black Carbon = 120 Gg** — at equilibrium (60 Gg/yr ÷ 0.5/yr). Old value of 600 Gg caused a sudden drop.

6. **Permafrost coefficient = 0.020** (not 0.037 — fixed from over-estimated global fraction).

7. **Melt Sensitivity = 0.47 km/yr/°C** (not 0.86 — recalibrated from Maurer et al. 2021 Nepal-specific data).

---

## Policy Scenarios to Test

| Scenario | Parameter Change | Key Outputs |
|----------|-----------------|-------------|
| BAU | No change | Baseline |
| Paris 1.5°C | Human Emission growth → −2%/yr after 2030 | ΔT, GHG, Glaciers Volume |
| BC Reduction | BC Emission growth → 0%/yr | Surface Albedo, Glacier Melt Rate |
| Forest Protection | Afforestation +0.014→0.022; Deforestation 0.012→0.006 | Forest Cover, GLOF Risk |
| Combined | Paris 1.5°C + BC Reduction | Best-case glacier trajectory |

---

## References

1. IPCC AR6 (2021) — GHG stock 830 Gt, emissions baseline, ECS 3°C
2. Myhre et al. (1998) — Logarithmic radiative forcing equation
3. Schuur et al. (2015) — Permafrost threshold 1.5°C, 1.7 Gt CO₂/°C/yr globally
4. Friedlingstein et al. (2022) — Ocean sink 10 Gt/yr, terrestrial 3.1 Gt/yr
5. ICIMOD HI-WISE (2023) — Nepal glacier volume 312 km³, area 3,902 km²
6. Maurer et al. (2021) — HMA melt −4.29 km³/yr; Nepal calibration
7. Stigter et al. (2021) — Yala/Rikha Samba mass balance; accumulation 55%
8. Brun et al. (2017) — HMA glacier mass balance; albedo reference values
9. Bolch et al. (2012) — Himalayan glacier state; altitude 4,500 m; albedo feedback
10. Rounce et al. / PNAS (2020) — 47 priority GLOF lakes; lake feed fraction 30%
11. Sherpa et al. (2020) — Khumbu precipitation 1,500 mm/yr; snow threshold 3°C
12. ICIMOD / ScienceDirect (2021) — BC 39% pre-monsoon melt; 3.8% albedo reduction
13. Nepal PMC (2024) — Forest carbon sequestration 2.5 Mt CO₂/yr
14. Global Forest Watch (2024) — Nepal forest 64,000 km²; 1.4%/yr afforestation
15. The Hindu (2024) — Glacial lake 10.81% area expansion 2011–2024
16. World Bank Nepal Climate Profile (2021) — Base temperature 14°C
17. Global Carbon Project (2022) — Human emission 36 Gt/yr in 1980; 1.8%/yr growth
18. Sterman (2000) — Business Dynamics, SD methodology
