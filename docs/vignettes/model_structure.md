---
layout: default
title: Model Structure
parent: Vignettes & Tutorials
nav_order: 2
---

# Model Structure: A Detailed Walkthrough

This vignette provides a step-by-step technical explanation of how MHASpread is structured—ideal for researchers and modelers.

---

## Overview

MHASpread is a **stochastic, multiscale, spatially explicit metapopulation model** of FMD transmission. Here we deconstruct its architecture.

---

## Level 1: The Farm (Local Dynamics)

### The SEIR Framework

Each farm maintains compartments for each species:

```
On Farm i, for Species s:

    S_i,s  →  E_i,s  →  I_i,s  →  R_i,s
    (Susceptible) (Exposed) (Infectious) (Recovered)
                ↓
            V_i,s (Vaccinated)
```

### Daily State Transitions

On day $t$, each farm updates:

1. **New infections**: $S \xrightarrow{\beta} E$ via mass-action
2. **Latency: $E \xrightarrow{1/\sigma} I$ after latent period
3. **Recovery**: $I \xrightarrow{1/\gamma} R$ after infectious period
4. **Demographic**: Birth/death rates applied
5. **Control**: Vaccination (if in zone), depopulation (if detected)

### Mathematical Formulation

The dynamics per species per farm follow differential equations:

$$\frac{dS}{dt} = u - v - \beta \frac{SI}{N}$$

$$\frac{dE}{dt} = \beta \frac{SI}{N} - v \cdot E - \frac{1}{\sigma}E$$

$$\frac{dI}{dt} = \frac{1}{\sigma}E - \frac{1}{\gamma}I - v \cdot I$$

$$\frac{dR}{dt} = \frac{1}{\gamma}I - v \cdot R$$

where $N = S + E + I + R + V$ (total animals).

---

## Level 2: Transmission Parameters

### The β Matrix (Species-Specific Transmission)

Central to MHASpread is the 3×3 transmission matrix:

|   | Cattle | Swine | Sheep |
|---|---|---|---|
| **Cattle (inf)** | β_cc | β_cs | β_cr |
| **Swine (inf)** | β_sc | β_ss | β_sr |
| **Sheep (inf)** | β_rc | β_rs | β_rr |

Example values (per animal per day):

|   | Cattle | Swine | Sheep |
|---|---|---|---|
| **Cattle (inf)** | 0.24 | 0.24 | 0.24 |
| **Swine (inf)** | 6.14 | 6.14 | 6.14 |
| **Sheep (inf)** | 0.105 | 0.024 | 0.105 |

**Interpretation**: If 1 infected swine is on a farm with 100 cattle, ~0.06 new cattle infections expected per day (6.14 × 1/100).

### Disease Duration Parameters

Species-specific to capture biological variation:

| Species | Latent Period (σ) | Infectious Period (γ) |
|---|---|---|
| Cattle | 3.6 days (range 2–5) | 4.4 days (range 3–6) |
| Swine | 3.1 days (range 2–4) | 5.7 days (range 5–6) |
| Sheep | 4.8 days (range 3–6) | 3.3 days (range 2–4) |

These are sampled from distributions (PERT beta-family) for each animal to preserve stochastic variation.

---

## Level 3: Between-Farm Transmission (The Kernel)

### The Spatial Kernel Function

Distance-dependent transmission probability for uninfected farm $j$:

$$P_E(t) = 1 - \prod_i \left[1 - \frac{I_i(t)}{N_i} \phi e^{-\alpha d_{ij}}\right]$$

**Breaking this down**:

1. **For each infected farm** $i$: Calculate transmission contribution
2. **Compute $\frac{I_i(t)}{N_i}$**: Infection prevalence (0 to 1)
3. **Multiply by $\phi e^{-\alpha d_{ij}}$**: Distance-decay function
4. **Multiply prevalence × decay**: Joint probability
5. **Product over all**: Combine contributions from all infected farms (exponential model)
6. **1 - product**: Convert to probability farm $j$ is exposed

### Kernel Parameters

- **$\phi = 0.044$**: Baseline transmission at zero distance
  - Low value reflects that spontaneous spread is rare
  - Most between-farm transmission via animal trade (kernel + events)

- **$\alpha = 0.6$ km$^{-1}$**: Decay rate
  - At 1 km: $e^{-0.6 \times 1} = 0.55$ (45% reduction)
  - At 5 km: $e^{-0.6 \times 5} = 0.05$ (95% reduction)
  - At 40 km: $e^{-0.6 \times 40}$ ≈ 0 (transmission negligible)

### Kernel Visual

```
Transmission Probability (relative to baseline)

1.0 |
    | ███
0.8 |██  ██
    |       ██
0.6 |         ██
    |           ██
0.4 |             ██
    |               ███
0.2 |                   ███
    |                      ███
0.0 |________________________███░░░░░░░
    0        5       10      15      20    40 km
    
██ = High transmission
░░ = Negligible transmission
```

---

## Level 4: Animal Movements (The Network Layer)

### Events File: Trade Network

Each movement event adds stochastic transmission pathway:

```
date: 2024-01-15
sender: Farm_001 (with 5 infected swine)
receiver: Farm_042 (all susceptible)
species: swine
count: 50 animals
```

### Transmission via Movement

Probability infection transmitted:

$$P(\text{transmission}) = 1 - \left(1 - \frac{I_{\text{sender}}}{N_{\text{sender}}}\right)^n$$

where $n$ = number of animals shipped.

**Example**: If 5 swine infected out of 100 total (5%), and 50 are shipped:
$$P = 1 - (0.95)^{50} \approx 0.92 \text{ (very likely to transmit)}$$

### Standstill Effect

When **movement standstill** active, movements from infected/buffer zones blocked:

- Cuts network transmission pathway
- Remains in place 30 days
- Enforced across simulation for all movements in zones

---

## Level 5: Detection & Diagnosis

### Surveillance Zones & Inspection

Active surveillance occurs in farms under surveillance. At iteration $i$:

**Number of farms inspected**:
$$k_i = \max(1, \lfloor P_i / 3 \rfloor)$$

Inspect at least 1 farm, or up to 1/3 of surveillance population.

### Hypergeometric Sampling

**Number of infected farms detected**:
$$E_i \sim \text{Hypergeometric}(M = P_i, n = I_i, N = k_i)$$

This accounts for:
- Finite surveillance population
- Sampling without replacement
- Realistic detection from limited inspections

### Diagnostic Sensitivity

Detected farms subject to test sensitivity:
$$E_i^* \sim \text{Binomial}(E_i, s)$$

If $s = 0.95$: 95% of infected farms with clinical signs are confirmed  
If $s = 0.80$: Only 80% detected (reflects subclinical cases)

---

## Level 6: Control Actions (The Intervention Layer)

### 6a. Depopulation

**Trigger**: Farm detected as infected  
**Action**: Culling of all animals on infected farms  
**Daily capacity**: Scenario-dependent (e.g., 3 farms/day)  
**Priority**: Largest herds (maximize animals removed)

Once depopulated, farm removed from simulation.

### 6b. Vaccination

**Trigger**: Farm in infected or buffer zone  
**Delay**: 15 days after zone establishment  
**Daily capacity**: Scenario-dependent (e.g., 5 farms/day)  
**Species**: Cattle only  
**Efficacy**: Scenario-dependent (e.g., 95%)

**Progression**: Each farm vaccinated over ~15 days:
$$\text{Animals vaccinated/day} = \lfloor N \times (1/15) \rfloor$$

Animals transitioned $S \to V$ (protected) following herd proportion.

### 6c. Movement Standstill

**Trigger**: Zone establishment  
**Duration**: 30 days  
**Scope**: Outgoing from infected/buffer, incoming to surveillance  
**Effect**: Blocks animal trade movements (cuts network transmission)

### 6d. Contact Tracing & Traceback

**Mechanism**: Identify farms that shipped to detected farm in past 30 days  
**Action**: Treat as detected infected, establish zones around traceback farms  
**Effect**: Expands control to connected farms upstream

---

## Level 7: Stochastic Simulation Loop

### Daily Iteration Algorithm

```
For each day t = 1, 2, ..., max_days:

  1. Within-farm transmission (all farms):
     For each species on each farm:
       New_E = Binomial(S, β × I / N)
       S → E for New_E animals

  2. Disease progression (all farms):
     For each animal, sample latent and infectious periods from distributions
     Apply transitions: E → I → R based on durations

  3. Between-farm transmission:
     Calculate P_E(t) for each uninfected farm
     Farms with exposure become infected (enter E compartment)

  4. Animal movements:
     Apply each movement event
     If sender infected, transmission to receiver with probability P(transmission)

  5. Detection:
     Sample hypergeometric for surveillance farms
     Apply diagnostic sensitivity
     Detected farms trigger control zones

  6. Control actions:
     Depopulate detected farms (if capacity allows, else queue)
     Vaccinate farms in zones (if zone active, lag passed, capacity allows)
     Block movements in standstill zones
     Trace back from detected farms

  7. Record state:
     Store S, E, I, R, V for all farms
     Record control actions taken
     Update zone status

End for
```

### Replicate Runs

Repeat algorithm 100–1000 times with same inputs:
- Different random seeds
- Different transmission outcomes
- Different detection patterns
- Different stochastic parameter values

**Result**: Distribution of outcomes (mean ± SD, percentiles)

---

## Level 8: Output Data

### Time Series by Farm

For each farm and day:

- $S_i(t)$, $E_i(t)$, $I_i(t)$, $R_i(t)$, $V_i(t)$ per species
- Total infected animals across farm
- Zone assignment (infected, buffer, surveillance, none)

### Epidemic Summary Metrics

Across full simulation:

- Total farms attacked (≥1 infected animal)
- Total animals infected (cumulative across all)
- Peak prevalence (max $I(t)$)
- Outbreak duration (days to elimination)
- Final size (total farms + animals affected)

### Control Action Summary

- Farms depopulated (timing)
- Animals vaccinated (number, efficacy)
- Movement events averted
- Farms detected via traceback

### Cost Inputs

If economic model linked:

- Depopulation costs (by farm, by animal)
- Vaccination costs
- Surveillance costs
- Lost productivity

---

## Key Assumptions Recap

| Assumption | Impact | Alternative |
|---|---|---|
| Homogeneous mixing within farm | May overestimate transmission if species segregated | Heterogeneous compartment division |
| Constant β (no season/age variation) | Simplified transmission | Time-varying or age-structured β |
| Perfect movement data | Assumes all trade recorded | Stochastic unobserved movements |
| Instant control execution | No delay in operations | Operational delay modeling |
| Single FMD serotype | No cross-strain complexity | Multi-strain model |
| Permanent immunity post-infection | No re-infection modeled | SEIRS with waning immunity |

---

## Computational Complexity

### Scalability

- **Small system** (50 farms, 1 month): ~seconds per replicate
- **Medium system** (500 farms, 3 months): ~minutes per replicate
- **Large system** (5000 farms, 1 year): ~hours per replicate × 100 replicates

### Parallelization

Replicates run independently → easily parallelized across cores

---

## Validation & Calibration

### How Do We Know the Model is Correct?

1. **Historical reconstruction**: Simulate known outbreaks (Rio Grande do Sul 2000–2001)
2. **Compare to observed**: Do simulated outcomes match field data?
3. **Sensitivity analysis**: Vary parameters, check robustness
4. **Expert review**: Epidemiologists validate assumptions

### Calibration Data

- FMD experimental transmission studies
- Field outbreak investigations
- Livestock census data
- Trade network analysis

---

## What's Next?

- For deeper dive into transmission mechanisms, see [Transmission Processes Vignette](transmission_processes.md)
- For control strategy implementation, see [Control Strategies Vignette](control_strategies.md)
- For case study walkthrough, see [Case Studies Vignette](case_studies.md)

