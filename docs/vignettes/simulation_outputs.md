# Simulation Outputs: Interpreting Results

This vignette explains how to understand, interpret, and visualize MHASpread simulation outputs—critical for evidence-based decision-making.

---

## Overview

MHASpread produces rich output data at multiple scales:

- **Farm-level time series**: Daily compartment counts (S, E, I, R, V) per species
- **Epidemic summary metrics**: Outbreak size, duration, peak prevalence
- **Spatial patterns**: Which farms attacked, where outbreak concentrated
- **Control action records**: Farms depopulated, animals vaccinated, movements blocked
- **Stochastic ensemble**: Results across 100–1000 replicate simulations

---

## Understanding the Output Structure

### Level 1: Single Replicate Time Series

Each simulation run generates daily output:

```
Day | Farm_001_I_bov | Farm_001_I_swi | Farm_002_I_bov | ... | Total_I_all
----|----------------|---|---|----|----|
 1  |        0       |  0 |  0  | ... |   0
 2  |        1       |  2 |  0  | ... |   3
 3  |        3       |  5 |  1  | ... |  9
 4  |        6       | 12 |  3  | ... | 21
 5  |       12       | 25 |  8  | ... | 45
...
20  |      250       | 450| 180 | ... |  880
21  |      180       | 380| 140 | ... |  700 (control begins)
...
50  |        5       |  2 |  0  | ... |  7
60  |        0       |  0 |  0  | ... |  0 (outbreak over)
```

### Level 2: Key Metrics Per Replicate

For each simulation run, extract:

| Metric | Definition | Example |
|---|---|---|
| **Final Size (farms)** | Total unique farms attacked | 47 |
| **Final Size (animals)** | Total animals infected | 8,350 |
| **Peak Prevalence** | Maximum infectious animals at any time | 1,200 |
| **Outbreak Duration** | Days from index to last recovery | 68 |
| **Attack Rate** | Farms attacked / total farms | 47/500 = 9.4% |
| **Farms Depopulated** | Number culled | 23 |
| **Animals Vaccinated** | Total vaccinated | 12,500 |

### Level 3: Ensemble Statistics

Run 100 replicates of same scenario → distribution of outcomes:

| Metric | Mean | Median | 5th % ile | 95th %ile |
|---|---|---|---|---|
| **Final Size (farms)** | 58 | 52 | 15 | 142 |
| **Peak Prevalence** | 950 | 880 | 200 | 2,100 |
| **Duration (days)** | 71 | 68 | 35 | 120 |

**Interpretation**: Median outcome is 52 farms; but 5% of scenarios will see <15 farms (best case), and 5% will see >142 farms (worst case, unlucky transmission).

---

## Epidemic Curves: The Visual Summary

### What is an Epidemic Curve?

Plot of **total infectious animals** vs. **time**:

```
Infectious Animals
      |
2,500 |        ╱╲
      |       ╱  ╲
2,000 |      ╱    ╲
      |     ╱      ╲
1,500 |    ╱        ╲
      |   ╱          ╲
1,000 |  ╱            ╲
      | ╱              ╲___
  500 |╱                   ╲__
      |                       ╲__
    0 |_________________________╲_____
      0    10    20    30    40    50  (Days)
            Silent    | Control Active
            Spread    |
```

### Interpreting the Curve

1. **Days 0–20**: Exponential rise (silent spread, no control)
   - Shows how fast infection spreads unimpeded
   - Steeper curve = faster transmission (swine-heavy regions)

2. **Days 20–25**: Detection & zone establishment (inflection point)
   - Curve still rising (detection lags)
   - But growth rate slowing (early control actions)

3. **Days 25–50**: Plateau & decline (control effective)
   - Peak prevalence marks turning point
   - Decline indicates control > new transmission

4. **Days 50+**: Tail-out (resolution)
   - Remaining infectious animals recover naturally
   - Outbreak ends when I = 0

### Multi-Strategy Comparison

Plot epidemic curves for different scenarios:

```
Infectious Animals
      |
2,500 |   Uncontrolled ════════╗
      |                        ║
2,000 |  Conservative ─────╗   ║
      |                    │   ║
1,500 | Moderate ──╗       │   ║
      |            │       │   ║
1,000 |Aggressive─╖        │   ║
      |           ║        │   ║
  500 |           ║        │   ║
      |           ║        │   ║
    0 |___________|________|___|_____
      0    10    20    30    40    50
            
Conservative: Peak ≈1500, Duration ≈45 days
Moderate:    Peak ≈800,  Duration ≈35 days
Aggressive:  Peak ≈300,  Duration ≈25 days
```

**Finding**: Aggressive strategy shortens outbreak by ~40%, but costs 2× more. Value judgement for policymakers.

---

## Spatial Visualization

### Farm Attack Map

Plot farms on geographic map; color by infection status:

```
Geographic Map (latitude vs. longitude)

   North
    ↑
    │ ◉ ◉ ■ ◉                 ◉ Uninfected farm
    │ ◉ ■ ■ ◉ ◉              ■ Infected farm
    │ ◉ ◉ ■ ◉ ◉              ★ Index farm
    │ ★ ◉ ■ ◉               
    │ ◉ ◉ ■ ◉ ◉
    └─────────────→ East
    
Infected zone (3 km): Concentrated cluster around index
Buffer zone (7 km):   Secondary cluster
Surveillance zone (15 km): Scattered cases
Beyond 15 km: Few cases (spatial kernel decay)
```

### Temporal Progression

Show infection spread over time as animation frames:

```
Day 0     Day 10     Day 20     Day 30
(Index)   (Silent)   (Detection)(Control)

  ◉        ◉ ■       ◉ ■ ■     ◉ ◉
  ◉ ★      ◉ ★ ■     ◉ ★ ■■    ◉ ◉
  ◉        ◉ ◉ ■     ◉ ◉ ■■    ◉ ◉
          
→ Visualization shows spatial wave moving outward
→ Control zones halt spatial progression
```

---

## Compartment Breakdowns

### By-Species Infection Dynamics

Track I compartment by species over time:

```
Infectious Animals by Species

     Cattle (blue) ───
     Swine (red) ─ ─ ─
     Sheep (green) ─·─

     │
 1500│            ╱──
     │       ╱───╱
 1000│  ╱───╱─
     │╱────╱┐
  500│     ╱ ╲
     │    ╱   ╲
     │___╱_____╲___
     0  10  20  30  40  (Days)

Interpretation:
- Swine peak first (days 8–15)
- Cattle peak later (days 10–20) after swine transmission
- Sheep remain low throughout (inefficient transmitters)
```

### Vaccination Effectiveness

Track V compartment growth in buffer zone:

```
Vaccinated Cattle in Buffer Zone

     │
1000│              ╱╱╱╱╱╱╱
     │           ╱╱╱
  800│         ╱╱╱       (100% coverage achieved)
     │       ╱╱╱
  600│     ╱╱╱
     │   ╱╱╱
  400│ ╱╱╱
     │╱╱╱              (Vaccination starts: day 35)
  200│
     │
    0│_____________________
     30   35    40   45   50  (Days)

Interpretation:
- Lag period (days 30–35): No vaccinations
- Rapid uptake (days 35–45): Daily capacity determines slope
- Plateau (days 45+): All eligible cattle vaccinated
- Effect: Susceptible population in buffer zone reduced 80–95%
```

---

## Comparative Analysis: Scenario Comparison Tables

### Standard Comparison Across Strategies

| Metric | Baseline | Scenario A | Scenario B | Scenario C |
|---|---|---|---|---|
| **Final farms** | 120 | 65 | 38 | 18 |
| **Peak I (animals)** | 2,100 | 1,200 | 650 | 280 |
| **Duration (days)** | 95 | 68 | 45 | 32 |
| **Attack rate** | 24% | 13% | 7.6% | 3.6% |
| **Cost (€k)** | 0 | 350 | 600 | 1,200 |
| **Cost per farm prevented** | — | €4,400 | €7,700 | €17,000 |

**Reading**: Scenario B offers best cost-effectiveness; Scenario C has diminishing returns.

---

## Stochastic Uncertainty Representation

### Ensemble Epidemic Curves (with Percentiles)

Plot single mean curve + shaded confidence band:

```
Infectious Animals (Ensemble of 100 replicates)

     │              95th percentile ········
     │                  ╱╲·················
     │                 ╱  ╲················
 1500│                ╱    ╲·······════════
     │               ╱      ╲····╱ median
 1000│              ╱        ╲·╱╱╱════════
     │             ╱          ╱╱ shaded band
  500│            ╱          ╱╱  (5th-95th %ile)
     │          ╱╱╱         ╱
     │       ╱╱╱          ╱
    0│___╱╱╱___________╱_________
     0    10    20    30    40    50

Interpretation:
- Central black line: median outcome across replicates
- Shaded region: range of outcomes (uncertainty)
- Wide shading = high uncertainty (stochasticity)
- Narrow shading = consistent outcomes
```

### Box Plots: Distribution of Key Metrics

Visualize full distribution of outcomes:

```
Final Size (Farms)

Scenario A                  Scenario B
  │                           │
  │ •··· outliers              │ •
  │ ╭─╮ upper quartile         │ ╭─╮
  │ │█│ median                 │ │███│
  │ ╰─╯ lower quartile         │ ╰─╯
  │ "│" whiskers               │
  │ :                          │ :
    0        100               0        50
    
Scenario A: Median=65, Range=15–180 (high variability)
Scenario B: Median=38, Range=10–95  (lower variability)

→ Scenario B is both smaller AND more predictable
```

---

## Cost-Effectiveness Outputs

### Cost Curves (Incremental Analysis)

Plot total cost vs. outbreak containment:

```
Total Cost (€M)

1.5 │              Scenario C
    │             ╱
1.0 │            ╱  Scenario B
    │           ╱  ╱
0.5 │          ╱  ╱  Scenario A
    │         ╱  ╱
0.0 │────────╱──╱────────────
    0    50   100   150 (Farms Prevented)
    
Cost-effectiveness frontier:
- Below line: dominated (worse outcomes, higher cost)
- On line: efficient (optimal for given budget)
- Scenario B typically on frontier
```

### Cost-Benefit Summary

```
COST-BENEFIT ANALYSIS

                    Scenario: Moderate
─────────────────────────────────────────
Control Cost:              €600,000
─────────────────────────────────────────
Uncontrolled Outbreak:     200 farms infected
Controlled Outbreak:        60 farms infected
Farms Prevented:           140 farms
─────────────────────────────────────────
Cost per Farm Prevented:   €4,286
Avg. Farm Value Loss:      €50,000
Benefit per Farm:          €50,000
Net Benefit:               €7,000,000
─────────────────────────────────────────
Benefit-to-Cost Ratio:     11.7:1
─────────────────────────────────────────

Interpretation: Every €1 spent on control 
saves €11.70 in prevented losses.
Very favorable return on investment.
```

---

## Sensitivity Analysis Visualization

### Tornado Diagram: Parameter Sensitivity

Show which parameters most affect outcome:

```
OUTBREAK SIZE SENSITIVITY TO PARAMETERS

Parameter                Impact on Final Size
─────────────────────────────────────────────

Depopulation speed       ████████████████ (large effect)
Detection lag            ███████████████  (large effect)
Vaccination efficacy     ████████         (moderate effect)
Movement standstill      ███████          (moderate effect)
Diagnostic sensitivity   ███              (small effect)
Kernel decay (α)         ██               (small effect)

→ Depopulation speed & detection lag drive outcomes
→ Focus preparedness investment on these
```

### Sensitivity Table: How Outcomes Change

| Parameter | Low | Base | High | Range |
|---|---|---|---|---|
| Depop capacity (farms/day) | 1 | 3 | 5 | 120→60 farms |
| Detection lag (days) | 1 | 3 | 7 | 35→95 farms |
| Vax efficacy (%) | 80 | 95 | 99 | 55→40 farms |
| Standstill duration (days) | 14 | 30 | ∞ | 90→50 farms |

**Finding**: Depopulation capacity most sensitive; invest there first.

---

## Output File Formats

### Common Export Formats

MHASpread typically outputs:

1. **CSV files**: Time series, summary metrics (easy to read in Excel)
2. **RDS files**: Complete R objects (for further analysis)
3. **JSON files**: Summary statistics (for web visualization)
4. **Figures**: Scenario plots (PNG, PDF for reports)

### Working with Output in R

```r
# Load results from single replicate
results <- readRDS("mhaspread_outbreak_rep1.rds")

# Extract time series
I_timeseries <- results$compartments$I_all

# Plot epidemic curve
plot(I_timeseries, main="Epidemic Curve")

# Extract summary metrics
final_size <- nrow(unique(results$infected_farms))
peak_I <- max(results$compartments$I_all)
duration <- which(results$compartments$I_all == 0)[1]
```

---

## Communication to Stakeholders

### For Government Ministers (Executive Summary)

**One-page output:**

```
MHASPREAD SCENARIO ANALYSIS: FMD PREPAREDNESS

Scenario:              Moderate Response
_________________________________________
Estimated Outbreak:    60 ± 25 farms infected
Peak Cases:            800 infectious animals
Duration:              35–45 days
Cost of Response:      €600,000
_________________________________________
OUTCOME: Containment likely; acceptable risk.

RECOMMENDATION: Adopt Moderate scenario.
Budget allocation: €600k for surge capacity.
```

### For Farming Community (Transparency)

**Clear visuals:**

- "Our response plan will detect 95% of infected farms within 3 days"
- "Vaccination zones will protect 80% of cattle in buffer areas"
- "Expected quarantine period: 30 days"

---

## Common Pitfalls in Interpretation

### Pitfall 1: "Single Replicate = Reality"

**Wrong**: Running MHASpread once and using that result.  
**Right**: Run 100+ replicates; report median & 95% range.

### Pitfall 2: "Median = Expected"

**Wrong**: Assuming median outcome will occur.  
**Right**: Prepare for worst-case (95th percentile); median is optimistic.

### Pitfall 3: "Model = Perfect Prediction"

**Wrong**: Believing model output is exactly what will happen.  
**Right**: Model is strategic guide; real outbreak has surprises (hidden infection, variant transmission, human behavior).

---

## Next Steps

- For case study application examples, see [Case Studies Vignette](case_studies.md)
- For economic integration, see [Economic Impact Framework](../docs/economic_impact.md)
- For model details, see [Model Overview](../docs/model_overview.md)

