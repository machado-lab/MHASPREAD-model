# Transmission Processes: How FMD Spreads

This vignette focuses on the mechanisms and dynamics of disease transmission—essential for understanding outbreak trajectory.

---

## Overview

Disease transmission in MHASpread occurs through three pathways:

1. **Within-farm**: Direct contact, environmental contamination
2. **Spatial**: Distance-dependent between neighboring Farms
3. **Trade network**: Animal shipments across large distances

Understanding each is critical for predicting outbreak severity.

---

## Within-Farm Transmission: The Danger of Swine

### The Transmission Hierarchy

A central finding from FMD research is that swine are **super-spreaders**:

|   | Transmission Efficiency | Relative to Cattle | Interpretation |
|---|---|---|---|
| Swine → anyone | ~6.14 animals⁻¹day⁻¹ | × 25 | Very efficient |
| Cattle → anyone | ~0.24 animals⁻¹day⁻¹ | × 1 | Baseline |
| Sheep → anyone | ~0.05–0.10 animals⁻¹day⁻¹ | × 0.2–0.4 | Inefficient |

### Why Swine Dominate Transmission

Three biological factors:

1. **Viral shedding**: Swine secrete 10–100× more virus in respiratory/fecal matter
2. **Housing density**: Confinement buildings create high-density conditions
3. **Environmental contamination**: Pig waste more extensively contaminates surroundings

**Practical implication**: A single infected swine farm can spark regional outbreaks if borders crossed.

### Example: A Mixed Farm Outbreak (Day-by-Day)

**Day 0**: 1 infected swine introduced (via animal trade or infection breach).

**Day 1–2**: Swine latent; farm appears normal.

**Day 3–4**: Swine infectious; co-located cattle at ~20% cumulative infection risk.

**Day 5**: ~5–10 cattle become infectious. Spatial kernel activates.

**Day 6–10**: Neighboring farms at elevated risk. Movement of cattle/swine from mixed farm spreads to distant locations.

**Day 15–20 (if undetected)**: Potentially 20–50 farm outbreak established across region.

**Impact**: Detection speed determines final outbreak size.

---

## Spatial Transmission: Distance as Protection

### How the Kernel Works

Spatially, FMD transmission follows **exponential distance decay**:

$$\text{Risk} \propto e^{-0.6 \times \text{distance}}$$

### Quantitative Effects

| Distance (km) | Transmission Risk Relative to 0 km | Expected Transmission Between Farms |
|---|---|---|
| 0 km (adjacent) | 100% | Moderate (~1–5% per day if source infected) |
| 1 km | 55% | ~0.5–2% per day |
| 3 km | 16% | ~0.1–0.5% per day |
| 5 km | 5% | ~0.05–0.1% per day |
| 10 km | 0.25% | Rare (~1–2 times full outbreak) |
| 20 km | 0.0062% | Essentially blocked unless animal moved |
| 40 km | ~0 | Transmission assumed zero |

### What Drives Spatial Spread?

**Primary mechanism**: Environmental transmission (airborne when wind direction favorable, fomite). 

**Why limited range?**

- Virus survives in environment but doesn't travel far
- Most contact is local (neighboring farms, shared water)
- Lack of direct animal contact across farm boundaries

---

## Trade Networks: The Long-Distance Amplifier

### Why Movements Matter

Animal trade is the **dominant driver of large-scale spread**. Consider two routes:

#### Route 1: Spatial Only (No Trade)
```
Day 0: Infected farm A
    ↓ (spatial kernel, slow)
Day 10: Neighboring farms (3–10 km radius)
    ↓ (slow expansion)
Day 30: ~20–50 farm outbreak, confined to region
```

#### Route 2: With Trade (Movement)
```
Day 0: Infected farm A
    ↓ (trade shipment)
Day 2: Farm B (100 km away) receives infected shipment
    ↓ (spatial + new trade hub)
Day 15: ~100–200 farm outbreak across regions
```

**Impact**: Movements can accelerate spread **10–100×**.

### Movement Patterns in Livestock

Three types of movements:

1. **Short-range** (< 30 km): Local trading, restocking, grazing contracts
2. **Medium-range** (30–200 km): Regional auctions, fattening operations
3. **Long-range** (> 200 km): Interstate trade, international (if allowed)

**FMD lesson**: One long-range movement can jump infection across geographical barriers.

### Example: Movement-Driven Cascade

**Day 0**: Farm A has 10 infected swine (purchased from endemic region).

**Day 5**: Farm A sells 50 swine to Farm B (100 km away).
- ~5–10 of shipment likely infected (Day 5 is days 3–4 infectious for index animal)
- ~90% transmission probability to recipient farm

**Day 10**: Farm B now infectious. Sells cattle to Auctions C and D.

**Day 12**: From Auctions, cattle shipped to Farms E, F, G, H, I (5 distant farms).

**Day 15**: If undetected, 5–6 farms now entering infectious phase.

**Day 30**: Without control, network-driven exponential growth possible.

**Key**: Early detection of Farm A (Day 1–2) stops cascade; late detection (Day 10+) unstoppable.

---

## Detection & Its Critical Role

### Detection Timing: The Bottleneck

**Silent spread (Days 0–20)**: Infection spreads undetected because:

- No visible signs on some farms (subclinical)
- Farmers unaware of FMD presence
- Surveillance not yet triggered

### Detection Triggers

1. **Clinical observation**: Farmer notices lesions, lameness, fever
2. **Active surveillance**: Routine veterinary inspections
3. **Traceback**: Tracing from detected case identifies prior movements

### Sensitivity & Specificity

**Diagnostic sensitivity** ($s$) affects detection:

- $s = 1.0$ (perfect): All infected farms detected when sampled
- $s = 0.95$: 5% of infected farms missed (subclinical, sampling error)
- $s = 0.80$: 20% missed

Even perfect testing only detects farms in surveillance zone (~1/3 sampled per day).

---

## Phase 1: The Silent Spread (Days 0–20)

### Uncontrolled Transmission

Pre-detection, all transmission pathways active:

- Within-farm: Full rate
- Spatial: Full kernel
- Movements: No restrictions

### What Happens in 20 Days?

**Low-transmission scenario** (cattle-only region, limited trade):
- ~10–20 farms infected
- Confined to ~30 km radius

**High-transmission scenario** (mixed cattle-swine, active trade):
- ~50–200 farms infected
- Spread across 100+ km (via long-distance movements)

### Variability

Stochastic outcomes mean outbreaks vary dramatically:

- **Best case** (lucky): ~5 farms, detection same region
- **Worst case** (unlucky): ~200 farms across multiple regions

**Implication**: Policy must be prepared for worst-case.

---

## Phase 2: After Detection (Control Active)

### Zone-Based Transmission Reduction

Once control zones established, transmission modified:

#### Infected Zone (3 km)
- **Depopulation**: Removes infected farms immediately
- **Effect**: Cuts within-farm + spatial transmission from zone

#### Buffer Zone (3–7 km)
- **Vaccination**: Cattle protected (cannot become infected)
- **Effect**: Reduces susceptible population; slows spread

#### Surveillance Zone (7–15 km)
- **Enhanced detection**: More frequent monitoring
- **Effect**: New cases detected faster → earlier zone establishment

### Movement Standstill Impact

30-day restriction on movements **from** infected/buffer zones:

- Blocks network transmission pathway
- Prevents long-distance jumps
- Allows local spatial spread to be contained

---

## Transmission Dynamics Under Different Scenarios

### Scenario A: Fast Depopulation (5 farms/day)

```
Day 20: 15 infected farms detected
Day 21–25: All depopulated (capacity sufficient)
Result: Outbreak contained, ~15 farms final size
```

**Why works**: Depopulation removes infection sources before spatial spread accelerates.

### Scenario B: Slow Depopulation (1 farm/day)

```
Day 20: 15 infected farms detected
Day 21–35: Only 15 depopulated (slow capacity)
Day 25–30: Uncontrolled depopulated farms still infectious
Result: Outbreak expands; ~80–100 farms final size (8× larger!)
```

**Why fails**: Logistical bottleneck allows spatial spread to dominate.

### Scenario C: Vaccination-Focused

```
Day 20: 15 infected farms detected
Day 35: Vaccination begins (15-day lag)
Day 35–45: Cattle herds vaccinated in buffer zone
Result: Spread contained but slower; ~40–60 farms
```

**Trade-off**: Preserves herds but slower epidemic control than depopulation.

---

## Key Insights for Policy

### 1. Early Detection Saves Lives (and Money)

Reducing silent spread by 1 week can reduce final outbreak size by 50–80%.

**Implication**: Invest in passive + active surveillance.

### 2. Depopulation Speed Critical for Cattle Regions

In regions with cattle only:
- Fast depopulation (5 farms/day) similar to vaccination
- Slow depopulation fails

**Implication**: Maintain surge capacity for culling infrastructure.

### 3. Vaccination Crucial for Mixed Farming

In regions with cattle + swine:
- Vaccination of cattle in buffer zone protects susceptible population
- Reduces spatial spread risk
- Complements depopulation

**Implication**: Emergency vaccine stockpiles essential in mixed regions.

### 4. Movement Control is Non-Negotiable

30-day standstill alone prevents 30–50% of outbreak expansion (via network).

**Implication**: Trade restrictions must be immediate and strict.

### 5. Context Matters

- Cattle-only regions: Prioritize depopulation
- Mixed regions: Combine depopulation + vaccination
- Trade-intensive regions: Emphasize network surveillance
- Dense farming areas: Spatial transmission dominates

---

## Stochastic Variability: Why Replicates Matter

### Same Inputs, Different Outcomes

Running MHASpread 100 times with identical parameters:

- **Best 10%**: ~20–30 farms infected (lucky early detection)
- **Median**: ~60–80 farms (typical)
- **Worst 10%**: ~150–200 farms (unlucky, late detection, network amplification)

### Implications

1. **Single-run simulation is misleading**: Must report distribution (median ± 95% range)
2. **Probability of success varies**: Control strategy X works 80% of time, but 20% of scenarios fail
3. **Planning for worst-case**: Policy should account for 90th percentile outcome

---

## Next Steps

- For details on control strategy timing, see [Control Strategies Vignette](control_strategies.md)
- For case study application, see [Case Studies Vignette](case_studies.md)
- For model mathematics, see [Model Overview](../docs/model_overview.md)

