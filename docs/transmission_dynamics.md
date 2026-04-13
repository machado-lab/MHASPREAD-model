---
layout: default
title: Transmission Dynamics
parent: Model
nav_order: 2
---

# Transmission Dynamics

This page details the specific mechanisms by which foot-and-mouth disease spreads within and between farms in the MHASpread framework.

---

## Overview of Transmission

Transmission of FMD in MHASpread occurs at two interconnected scales:

1. **Within-farm transmission**: Direct and indirect contact among housed animals
2. **Between-farm transmission**: Distance-driven environmental spread and animal trade

---

## Within-Farm Transmission

### Mass-Action Dynamics

Infection spread within farms follows classical mass-action transmission:

$$\text{Force of Infection}_i = \frac{S_i(t) \cdot I_i(t) \cdot \beta_i}{N_i}$$

This formulation assumes:

- **Homogeneous mixing**: Any susceptible is equally likely to contact any infectious animal
- **Frequency-dependent contact**: Transmission rate scales with density of both susceptible and infectious animals
- **Proportional risk**: Risk is $\propto \frac{I}{N}$ (infection prevalence)

### Species-Specific Transmission: The Role of Swine

A defining feature of FMD epidemiology is the disproportionate role of swine as disease amplifiers:

#### Transmission Efficiency Hierarchy

**Swine >> Cattle >> Small Ruminants**

Quantitatively:

- **Swine-to-cattle**: $\beta \approx 6.14$ animal$^{-1}$ day$^{-1}$ (mode)
- **Cattle-to-cattle**: $\beta \approx 0.24$ animal$^{-1}$ day$^{-1}$ (mode)
- **Small ruminant-to-cattle**: $\beta \approx 0.105$ animal$^{-1}$ day$^{-1}$ (mode)

**Implication**: A single infected swine can infect ~25× more cattle individuals per day than an infected cow.

#### Biological Basis

Differential transmissibility reflects:

1. **Viral shedding rates**: Swine shed 10–100× higher viral loads (nasopharyngeal, fecal)
2. **Exposure duration**: Swine remain highly infectious for ~6 days vs. cattle ~4 days
3. **Aerosol generation**: Swine in confined housing generate more respiratory particles
4. **Fomite transmission**: Swine waste contaminates environment more extensively

### Temperature and Environmental Persistence

While not explicitly modeled as a time-varying parameter, environmental FMD persistence supports assumption that:

- **Same-farm exposure over hours/days** is realistic
- **Within-farm transmission coefficients** implicitly include environmental contribution

This is why $\beta$ values are relatively high compared to direct-contact-only models—they represent both direct and indirect (environmental) transmission.

---

## Between-Farm Transmission

### The Spatial Transmission Kernel

The exponential transmission kernel models local disease spread:

$$P_{E_j}(t) = 1 - \prod_i \left(1 - \frac{I_i(t)}{N_i} \phi e^{-\alpha d_{ij}}\right)$$

### Kernel Components: What Each Parameter Does

#### Baseline Transmission ($\phi = 0.044$)

At zero distance ($d_{ij} = 0$), probability of between-farm transmission per infectious animal is $\phi \cdot \frac{I_i}{N_i}$. 

- If farm $i$ is at ~10% prevalence and immediately adjacent to farm $j$:
- $P(\text{transmission}) \approx 1 - (1 - 0.044 \times 0.1) = 0.0044$ per day

This reflects:

- **Rarity of spontaneous spread**: FMD typically moves through animal contact/trade, not spontaneously across farms
- **Environmental or fence-line contact**: Occasional but not dominant mechanism

#### Decay Parameter ($\alpha = 0.6$ km$^{-1}$)

Controls how rapidly transmission drops with distance:

$$P(\text{transmission at distance } d) \propto e^{-0.6d}$$

| Distance (km) | Relative Risk | Interpretation |
|---|---|---|
| 0 | 1.0 | Baseline |
| 1 | 0.55 | ~45% reduction per km |
| 3 | 0.16 | ~84% reduction at 3 km |
| 5 | 0.05 | ~95% reduction at 5 km |
| 10 | 0.0025 | ~99.75% reduction at 10 km |
| 15 | 0.0001 | ~99.99% reduction at 15 km |

### Maximum Distance Cutoff (40 km)

Transmission negligible beyond 40 km, grounded in:

- **Empirical outbreak data**: FMD rarely spreads >40 km without animal movements
- **Mathematical models**: Spatial analysis of historic epidemics (Björnham et al., 2020)
- **Biological plausibility**: Environmental FMD survival cannot sustain spread over >40 km

### Distance Metric

Distances are **Euclidean (straight-line)** rather than network distance:

$$d_{ij} = \sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}$$

This assumes:

- Sufficient spatial data resolution (farms mapped at km scale)
- Landscape homogeneity (terrain doesn't strongly impede transmission)

**Refinement possible**: Using road-network distances for fine-scale simulation.

---

## Trade-Network Transmission

### Movement Events

Animal movements are the dominant driver of large-scale FMD spread. MHASpread incorporates an events database specifying:

- **Date**: Calendar day of shipment
- **Sender**: Origin farm
- **Receiver**: Destination farm
- **Species**: Cattle, swine, or small ruminant
- **Count**: Number of animals

### Movement-Driven Transmission Probability

When infectious animals are shipped:

$$P(\text{transmission via movement}) = P(\text{shipment includes infectious}) \times P(\text{transmit to receiver})$$

This is implicitly high (often >50–90%) if shipper is infected—hence movements are a critical accelerant of spread.

### Control Through Standstill

A 30-day movement restriction prevents all outgoing movements from infected and buffer zones, effectively impeding network-mediated transmission. This is why standstill is a central MHASpread control action.

---

## From Exposure to Disease

### Timeline: Latent Period

Once a farm becomes exposed (receives infectious material), animals transition through a latency phase during which:

- **Infection is established** but not yet producing virions
- **Animals are not yet infectious** to other animals
- **Duration**: Species-dependent (mean 2–5 days)

The latent period is sampled from empirically-derived distributions:

$$\sigma \sim \text{Distribution}(\text{species-specific mean, IQR})$$

**Impact on dynamics**: Latency creates a ~2–5 day delay between exposure and active transmission, allowing time for detection and control action.

### Infectious Period

Once animals develop clinical or subclinical infection, they shed virus and transmit:

- **Duration**: Species-dependent (mean 3–6 days)
- **Peak shedding**: Days 2–4 of clinical disease
- **Transmission**: Active throughout (no distinction between clinical/subclinical)

### Recovery

After infectious period, animals recover and are assumed to develop durable immunity:

$$I \xrightarrow{\gamma} R$$

**Assumption of durability**: In reality, reinfection with heterologous serotypes possible but not modeled here.

---

## From Import to Farm-Level Spread

### Example: How a Single Infected Shipment Cascades

**Day 0** (Import): 50 infected swine arrive at recipient farm with 200 cattle, 100 swine, 50 sheep.

**Day 1–3** (Latency): Swine are infected but not yet transmitting. Recipient farm appears seronegative.

**Day 3–4** (Infectiousness emerges): Swine begin shedding. Within-farm transmission kernel activates:

- Swine-to-cattle transmission: $\beta \approx 6.14$
- Many cattle become exposed

**Day 6–7** (Cattle infectious): First cattle become infectious. Between-farm kernel activates:

- Neighboring farms within 3 km have elevated exposure probability
- Movements to other locations may occur before detection

**Day 10–15** (Exponential phase): If undetected, neighboring farms become infected, spatial wave progresses.

---

## Heterogeneity in Susceptibility

### Farm-Level Differences

Transmission dynamics vary by farm context:

| Farm Type | Transmission Rate | Reason |
|---|---|---|
| **Single species (cattle)** | Baseline | Homogeneous, low amplification |
| **Mixed cattle/swine** | 5–10× faster | Swine amplification |
| **Large swine farm** | Highest | Density + shedding |
| **Smaller operations** | Lower | Reduced density, limited contact |

MHASpread captures this through within-farm size ($N$) and species composition but assumes homogeneous mixing once animals are on-site.

### Detection Heterogeneity

Infection detection varies by:

- **Farm inspection intensity** (more inspections → higher detection probability)
- **Diagnostic sensitivity** (clinical disease easier to detect than subclinical)
- **Farmer perceptiveness** (farm owner's ability to recognize disease)

Modeled via hypergeometric sampling and diagnostic sensitivity parameter $s$.

---

## Transmission in the Context of Control Zones

### Phase 1: Uncontrolled Spread (Pre-Detection)

20-day silent spread phase simulates:

- Unrestricted transmission
- No control actions
- Virus establishes across unknown number of farms
- Movement still occurs (no standstill yet)

**Output**: Realistic distribution of epidemic sizes and farm networks affected before awareness.

### Phase 2: Detection and Zone Establishment

Once infected farms are detected:

1. **Infected zone** (3 km) centered on detected farm
2. **Buffer zone** (7 km) surrounding infected zone
3. **Surveillance zone** (15 km) outer perimeter

Transmission continues but is progressively restricted:

- **Movements blocked**: Standstill prevents shipment-mediated transmission
- **Depopulation**: Reduces $I$ (infectious animals) in zone
- **Vaccination**: Converts cattle from $S \to V$ (protected)

### Phase 3: Containment and Resolution

If control actions are effective:

- $I$ decreases (from depopulation and recovery)
- $E$ decreases (latent animals mature but infectious pool reduced)
- Spatial kernel indicates no new exposures (few infectious farms remain)

**Containment success** depends on depopulation speed, vaccination coverage, and detection sensitivity.

---

## Stochasticity in Transmission

### Why Randomness Matters

Three sources of stochasticity in transmission:

1. **Binomial sampling**: Actual number of exposed individuals from force of infection (sampling error)
2. **Parameter variation**: $\beta$, $\sigma$, $\gamma$ drawn from distributions, reflecting biological uncertainty
3. **Spatial randomness**: Which farms are nearby (subset of population randomly contacted)

### Consequence: Outbreak Variability

Starting from identical initial conditions, replicate simulations can yield:

- **Complete fadeout**: Epidemic dies before large spread (5–10% of replicates in some scenarios)
- **Rapid expansion**: Exponential spread to dozens of farms
- **Variable duration**: 30–300 days depending on control effectiveness

**Implication**: Policy evaluation must use ensemble statistics (median, percentiles) rather than single deterministic simulation.

---

## Comparison to Deterministic Models

| Aspect | Deterministic | MHASpread (Stochastic) |
|---|---|---|
| Transmission | Force of infection computes exact new infections | Binomial sampling introduces chance |
| Parameters | Fixed to mean values | Sampled from distributions |
| Replicates | Single simulation | 100–1000 replicates |
| Outcomes | All identical | Distribution of outcomes |
| Extinction risk | Only from explicit control | May occur by chance |
| Computational cost | Very fast | Moderate (minutes to hours per scenario) |

**Trade-off**: Stochasticity adds realism but requires ensemble analysis.

---

## Real-World Calibration

### Data Sources for Transmission Parameters

1. **Experimental infections** (lab studies): Direct measurement of $\beta$, recovery kinetics
2. **Field outbreak data** (active surveillance): Documented secondary attack rates, spread patterns
3. **Retrospective reconstruction**: Analysis of known epidemics (Rio Grande do Sul 2000–2001)
4. **Comparative epidemiology**: Meta-analyses across global FMD events

### Uncertainty Quantification

MHASpread explicitly represents parameter uncertainty through:

- **PERT (Program Evaluation and Review Technique) distributions**: Beta-family distributions with specified mode and range
- **Sensitivity analysis**: Outer and inner parameter bounds tested
- **Ensemble inference**: Bayesian approaches integrating simulation and field data

This ensures that model outputs reflect not just simulation uncertainty but also fundamental epidemiological uncertainty.

---

## Next Steps

- For control strategies affecting transmission, see **[Control Strategies](control_strategies.md)**
- For practical application, see **[Transmission Processes Vignette](../vignettes/transmission_processes.md)**
- For data inputs, see **[Data Requirements](data_requirements.md)**

