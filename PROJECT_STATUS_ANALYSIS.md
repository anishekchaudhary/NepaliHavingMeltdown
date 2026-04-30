# Nepal Glacier System Dynamics Project — Status Analysis

## 🎯 PROJECT OVERVIEW
**Title:** System Dynamics Modeling of the Effect of Greenhouse Gases on Glaciers (Nepal Focus)

**Team:** 
- Anishek Kumar Chaudhary (24M0787)
- Pratik Man Shah (24M0856)
- Ritika Jaiswal (24M0855)
- Rohit Agrawal (24M0744)

**Course:** IE604 / CS752 System Dynamics (IIT Bombay)

**Deliverable Deadline:** Final Report + Vensim Models + Presentation

---

## 📋 CURRENT STATUS BREAKDOWN

### ✅ COMPLETED WORK (70% done)

#### 1. **Conceptualization & Qualitative Mapping**
- ✅ Problem definition: Greenhouse gas impact on Himalayan glaciers → downstream water security & flood risk
- ✅ Reference mode established: Historical glacier mass balance data 2000–2024 with reference to Brun et al. (2017) & ICIMOD HI-WISE
- ✅ Dynamic hypothesis defined: Multi-loop feedback system including:
  - Ice-albedo reinforcing loop
  - Permafrost thaw tipping point
  - Black carbon acceleration
  - GLOF undercutting dynamics
  - Carbon sink saturation

#### 2. **Causal Loop Diagrams (CLDs) — COMPLETE**
- ✅ **R1 — Ice-Albedo Feedback** (Reinforcing): Core warming acceleration loop
- ✅ **R2 — GLOF Undercutting** (Reinforcing): Glacial lake → thermal erosion → melt feedback
- ✅ **R3 — Permafrost Trap** (Reinforcing): Temperature threshold (1.5°C) triggers methane release
- ✅ **R4 — Black Carbon Melt** (Reinforcing): BC reduces surface albedo by up to 3.8%
- ✅ **B1 — Ocean Carbon Sink** (Balancing): Dynamic saturation with log-decay
- ✅ **B2 — Forest Carbon Sink** (Balancing): Nepal community forestry mitigation
- ✅ **B3 — GLOF Deforestation** (Balancing-to-Reinforcing): Floods → forest loss → sink degradation

#### 3. **Stock & Flow Diagram (SFD) — COMPLETE**
Located in: `Nepal_SFD.mdl` (Vensim format)

**Stock Structure:**
- Atmospheric GHG (with differentiated CO₂, CH₄, BC)
- Total Glacier Ice Mass
- Glacial Lake Volume
- Permafrost Carbon
- Forest Cover
- Runoff accumulation

**Flow Structure:**
- GHG Emissions (anthropogenic) + Permafrost Emissions + BC sources
- Climate forcing calculation (logarithmic)
- Glacier melt rate (temperature-dependent)
- Accumulation (precipitation, humidity-adjusted)
- Lake inflow/outflow and GLOF risk
- Forest sequestration & deforestation

#### 4. **Mathematical Model Implementation — 85% COMPLETE**
- ✅ Logarithmic climate forcing: ΔF = 1.5 × ln(GHG / Initial GHG)
- ✅ Temperature response calculation with albedo feedback
- ✅ Permafrost threshold logic: triggers at ΔT > 1.5°C, emits 1.7 Gt CO₂/°C/yr
- ✅ Black carbon dynamics: Atmospheric lifetime ~2 years, albedo reduction 3.8% max
- ✅ Glacial lake dynamics: Thermal undercutting + meltwater inflow
- ✅ Carbon cycle with differentiated sinks:
  - Ocean: 10 Gt/yr max with saturation (Friedlingstein 2022)
  - Terrestrial: 3.1 Gt/yr with BC-driven loss
  - Forest: 39 t/km²/yr in Nepal
- ✅ Unit checking: All variables dimensionally consistent
- ⚠️ Model testing: Basic logic verified; sensitivity testing partially completed

#### 5. **Model Documentation — 95% COMPLETE**
- ✅ Comprehensive HTML documentation with:
  - 14 subsystems organized by domain (Climate, Glacial, Hydrological, Carbon, Black Carbon)
  - 35+ variables with equations, units, and source citations
  - Feedback loop reference matrix
  - Parameter table with all values and justifications
  - Full reference list (18 sources) with tagged relevance

---

### ⚠️ IN PROGRESS (15% effort needed)

#### 1. **Policy Scenario Design & Sensitivity Analysis** (30% done)
**Completed:**
- Base case parameter set defined (emission growth 1.8%/yr, initial conditions from 1980)
- Permafrost threshold logic (1.5°C trigger point)

**Needed:**
- Design 2+ policy scenarios:
  - **Policy A:** Aggressive emission reduction (−3%/yr from 2025 onward)
  - **Policy B:** Accelerated afforestation (increase forest cover +5% by 2050)
  - **Policy C:** Combined mitigation strategy
- Run sensitivity analysis: ±20% parameter variations on key inputs:
  - Initial GHG concentration
  - Permafrost threshold temperature
  - Forest sequestration rate
  - BC emission growth rate
- Generate comparison graphs: Base Case vs. Policy 1 vs. Policy 2

#### 2. **Model Validation & Testing** (20% done)
**Completed:**
- Vensim "Units are OK" check: ✅ Passed
- Dimensional analysis: ✅ All consistent

**Needed:**
- Stress tests: Set key stocks to zero, verify logical behavior
- Parameter sensitivity plots (1D & 2D)
- Historical calibration: Compare base case output (2000–2024) against actual glacial mass loss data
- Extreme scenario testing: What happens with 0 emissions? With 2× BC?

---

### ❌ NOT YET STARTED (15% effort needed)

#### 1. **Final Report Writing** (0% started)
**Requirements (per course format):**
- **Section 1 — Summary:** 250 words max
- **Section 2 — Problem Definition & Reference Mode:** 300 words + graph
- **Section 3 — Qualitative Mapping:** 700 words + CLD image + feedback analysis
- **Section 4 — Model Formulation:** 1,000 words + SFD screenshot + 3 key equations + parameter table + testing results
- **Section 5 — Policy Analysis & Results:** 1,000 words + 3 scenario graphs + sensitivity results
- **Section 6 — Conclusions & Recommendations:** 500 words
- **Total:** 3,500–4,500 words (excluding appendices)

#### 2. **Presentation Preparation** (0% started)
- PowerPoint deck (estimated 15–20 slides)
- 10–15 minute presentation with visual demos
- Key metrics & graphs for audience engagement

#### 3. **Final Appendices** (0% started)
- Full equation export from Vensim (`Model > Document`)
- All prior reports (CLD report + Intermediate submission) included
- Contribution matrix signed by all 4 team members

---

## 📊 WORK DISTRIBUTION ESTIMATE

| Task | A | B | C | D | Status |
|------|---|---|---|---|--------|
| Problem definition & Reference modes | ✅ | ✅ | ✅ | ✅ | Complete |
| Causal Loop Diagrams | ✅ | ✅ | ✅ |   | Complete |
| Stock & Flow Diagram | ✅ |   | ✅ | ✅ | Complete |
| Equation writing & unit checking | ✅ | ✅ |   | ✅ | 85% |
| Documentation (HTML) |   | ✅ | ✅ |   | 95% |
| Sensitivity & policy scenarios |   | ⏳ |   | ⏳ | 30% |
| Model validation & testing | ⏳ | ✅ |   | ⏳ | 20% |
| **Report writing** | ❌ | ❌ | ❌ | ❌ | 0% |
| **Presentation prep** | ❌ | ❌ | ❌ | ❌ | 0% |
| **Appendices** | ❌ | ❌ | ❌ | ❌ | 0% |

---

## 🚀 IMMEDIATE NEXT STEPS (Priority Order)

### Phase 1: Finalize Model (2–3 days)
1. **Complete sensitivity analysis** in Vensim:
   - Create control panel sliders for key parameters
   - Run ±20% tests on: GHG initial, permafrost threshold, afforestation rate, BC growth
   - Export graphs for report

2. **Design & run policy scenarios**:
   - Scenario 1 (Base): Current trends through 2100
   - Scenario 2 (Emission Cut): Global emissions −3%/yr from 2025
   - Scenario 3 (Adaptation): Aggressive forestation + moderate emission control
   - Generate overlay plots (Glacier Mass, Temperature, Lake Volume, Runoff)

3. **Validation checkpoint**:
   - Verify model output 2000–2024 matches historical glacier loss (−0.80 m w.e./yr from Stigter 2021)
   - Document any calibration adjustments needed

### Phase 2: Report Writing (3–4 days)
1. **Section 1 (Summary):** Synthesize problem + loops + best policy (250 words)
2. **Section 2 (Prob Def):** Boundary + reference mode graph + dynamic hypothesis (300 words)
3. **Section 3 (CLD):** Export diagram, explain 3 key loops with plain-English logic (700 words)
4. **Section 4 (SFD & Equations):** Screenshot SFD, detail 3 critical equations (GHG temp effect, glacier melt, permafrost), parameter table with sources (1,000 words)
5. **Section 5 (Policy Results):** 3 scenario graphs + sensitivity table + comparison (1,000 words)
6. **Section 6 (Conclusions):** Learning insights + unintended consequences + recommendations (500 words)
7. **Formatting:** Apply H1/H2 headings, embed all graphs with captions, ensure 3,500–4,500 word range

### Phase 3: Final Deliverables (1–2 days)
1. **Export full equation list** from Vensim
2. **Compile appendices:** CLD report + Intermediate report + new material
3. **Create contribution matrix** with signatures
4. **Prepare PowerPoint:**
   - 1–2 slides: Problem & motivation
   - 2–3 slides: CLD with feedback loop explanations
   - 2–3 slides: Model structure (SFD overview)
   - 3–4 slides: Key equations & parameters
   - 3–4 slides: Policy results (comparison graphs)
   - 2–3 slides: Conclusions & policy recommendations
   - Q&A slide

---

## 🎓 KEY STRENGTHS OF YOUR MODEL

1. **Multi-scale feedback integration:** Captures climate physics, glacial processes, hydrological transitions, and human interventions in one coherent structure
2. **Empirically grounded:** All parameters sourced from peer-reviewed literature (IPCC, Nature, Nature Geoscience, ICIMOD)
3. **Tipping point physics:** Permafrost threshold creates realistic non-linear acceleration behavior
4. **Plausible policy levers:** Mitigation (afforestation, emission control) modeled with dynamic constraints
5. **Clear documentation:** Well-organized HTML reference + Vensim implementation
6. **Nepal-specific calibration:** Uses local data (glacier mass, black carbon, forest cover) rather than generic globals

---

## ⚠️ RISKS & MITIGATION

| Risk | Impact | Mitigation |
|------|--------|-----------|
| Sensitivity tests incomplete by deadline | Lose points on Policy Analysis section (1,000 words) | Start immediately; prioritize top 3 parameters |
| Report too long/short | Grade deduction | Write sections in parallel; use word counter frequently |
| Model output doesn't match historical data | Lose credibility; suggests tuning issues | Run quick calibration check this week |
| Unequal team contribution | Affects final grade + contribution matrix | Use matrix template to assign sections; track hours |
| Presentation lacks clarity | Audience confusion; poor delivery score | Draft slides from finished report; practice timing |

---

## 📁 FILE INVENTORY

| File | Status | Purpose |
|------|--------|---------|
| `Nepal_CLD.mdl` | ✅ Complete | Causal Loop Diagram (Vensim) |
| `Nepal_SFD.mdl` | ✅ Complete | Stock & Flow Diagram (Vensim) |
| `Model_Documentation.html` | ✅ 95% | Detailed variable reference + equations + sources |
| `SystemDynamicsInterm.pdf` | ✅ Complete | Intermediate project submission (now Appendix A) |
| `FinalSubmissionReprotFormat.pdf` | 🔄 In Progress | Final report (needs sections 1–6 + appendices) |
| `todo.md` | 📋 Reference | Original task checklist (now superseded) |

---

## ✨ SUMMARY SCORECARD

| Component | Target | Current | Gap |
|-----------|--------|---------|-----|
| **Conceptualization** | 100% | 100% | ✅ Done |
| **CLD & Feedback Analysis** | 100% | 100% | ✅ Done |
| **SFD & Equations** | 100% | 85% | 2–3 days |
| **Policy Scenarios** | 100% | 30% | 3–4 days |
| **Report Writing** | 3,500–4,500 words | 0 words | 4–5 days |
| **Presentation** | 15–20 slides | 0 slides | 1–2 days |
| **Final Appendices** | 100% | 10% | 1 day |
| **Overall Readiness** | 100% | **70%** | **~10 working days** |

---

## 🎯 CRITICAL SUCCESS FACTORS

1. ✅ **Model logic is sound:** Your feedback loops are realistic and well-grounded in physics
2. ⏳ **Complete sensitivity testing:** Must show parameter robustness (next 2 days)
3. ⏳ **Design compelling policies:** Scenarios must show realistic trade-offs (next 3 days)
4. ⏳ **Professional report narrative:** Explain *why* each model choice matters (next 4 days)
5. ⏳ **Polished presentation:** Visually communicate findings to non-specialists (next 2 days)

**Timeline to completion:** **10–12 working days** if work is distributed and paced well.

---

**Last Updated:** April 30, 2026  
**Next Review:** After sensitivity analysis completion
