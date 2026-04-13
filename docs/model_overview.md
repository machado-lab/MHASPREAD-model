---
layout: default
title: Model Overview
parent: Model
nav_order: 1
---

# Model Overview

This page provides a comprehensive technical description of the MHASpread model architecture, from compartmental structure through multiscale integration and stochastic processes.

---

## Model Architecture

MHASpread integrates disease dynamics across two coupled spatial scales:

### Scale 1: Within-Farm Dynamics (Local)

Individual farms contain multiple host species with homogeneous mixing. Disease progression follows a stochastic SEIR model with species-specific parameters.

### Scale 2: Metapopulation Dynamics (Regional)

Farms are embedded in a spatial landscape. Infection spreads between farms via two mechanisms:

1. **Spatial transmission**: Distance-dependent kernel (local environmental spread)
2. **Animal movements**: Trade shipments and restocking (network-mediated spread)

---

## Compartmental Structure

Each farm maintains disease state tracking for three host species: cattle, swine, and small ruminants.

### Core Compartments (Per Species Per Farm)

| Compartment | Symbol | Meaning | Transitions |
|---|---|---|---|
| **Susceptible** | $S$ | Uninfected, infection-capable animals | $\xrightarrow{\beta}$ to Exposed |
| **Exposed** | $E$ | Infected, non-infectious (incubation) | $\xrightarrow{1/\sigma}$ to Infectious |
| **Infectious** | $I$ | Infected, infectious animals | $\xrightarrow{1/\gamma}$ to Recovered |
| **Recovered** | $R$ | Immune (post-infection) | Remains (temporary/permanent immunity) |
| **Vaccinated** | $V$ | Vaccine-protected animals | Protected (immunity dependent on efficacy) |

### Total Population

$$N_i(t) = S_i + E_i + I_i + R_i + V_i$$

---

## Within-Farm Transmission Model

Disease progression within farm $i$ at time $t$ follows a stochastic discrete-time model based on differential equations:

### Susceptible Dynamics

$$\frac{dS_i(t)}{dt} = u_i(t) - v_i(t) - \frac{S_i(t)I_i(t)\beta_i}{N_i}$$

- $u_i(t)$ = animal births entering $S$ compartment
- $v_i(t)$ = animal deaths exiting all compartments
- $\frac{S_i(t)I_i(t)\beta_i}{N_i}$ = newly exposed animals (mass-action transmission)
- $\beta_i$ = within-farm transmission coefficient

### Exposed Dynamics

$$\frac{dE_i(t)}{dt} = \frac{S_i(t)I_i(t)\beta_i}{N_i} - v_i(t)E_i(t) - \frac{E_i(t)}{\sigma}$$

- First term: new exposures from $S$
- Second term: demographic exit
- Third term: progression to infectious (latent period = $\sigma$)

### Infectious Dynamics

$$\frac{dI_i(t)}{dt} = \frac{E_i(t)}{\sigma} - \frac{I_i(t)}{\gamma} - v_i(t)I_i(t)$$

- First term: progression from $E$
- Second term: transition to recovered (infectious period = $\gamma$)
- Third term: demographic exit

### Recovered Dynamics

$$\frac{dR_i(t)}{dt} = \frac{I_i(t)}{\gamma} - v_i(t)R_i(t)$$

---

## Species-Specific Parameters

### Transmission Coefficients (β)

The transmission coefficient $\beta$ is a 3×3 matrix reflecting transmission from each infected species to each susceptible species:

| Infected Species | Susceptible Species | $\beta$ Distribution | Basis |
|---|---|---|---|
| Cattle | Cattle | PERT(0.18, 0.24, 0.56) | Empirical (Rio Grande do Sul 2000–2001) |
| Cattle | Swine | PERT(0.18, 0.24, 0.56) | Assumed |
| Cattle | Small ruminants | PERT(0.18, 0.24, 0.56) | Assumed |
| Swine | Cattle | PERT(3.7, 6.14, 10.06) | Literature (Eblé et al., 2006) |
| Swine | Swine | PERT(3.7, 6.14, 10.06) | Literature (Eblé et al., 2006) |
| Swine | Small ruminants | PERT(3.7, 6.14, 10.06) | Assumed (Eblé et al., 2006) |
| Small ruminants | Cattle | PERT(0.044, 0.105, 0.253) | Assumed (Orsel et al., 2007) |
| Small ruminants | Swine | PERT(0.006, 0.024, 0.09) | Literature (Goris et al., 2009) |
| Small ruminants | Small ruminants | PERT(0.044, 0.105, 0.253) | Literature (Orsel et al., 2007) |

**Key observation**: Swine transmit FMD ~15–25× more efficiently than cattle and ~100× more efficiently than small ruminants, reflecting empirically observed differences in viral shedding and susceptibility.

### Disease Duration Parameters

| Parameter | Species | Mean (Median, IQR) | Unit | Reference |
|---|---|---|---|---|
| **Latent period** ($\sigma$) | Cattle | 3.6 (3, 2–5) | days | Mardones et al., 2010 |
| | Swine | 3.1 (2, 2–4) | days | Mardones et al., 2010 |
| | Small ruminants | 4.8 (5, 3–6) | days | Mardones et al., 2010 |
| **Infectious period** ($\gamma$) | Cattle | 4.4 (4, 3–6) | days | Mardones et al., 2010 |
| | Swine | 5.7 (5, 5–6) | days | Mardones et al., 2010 |
| | Small ruminants | 3.3 (3, 2–4) | days | Mardones et al., 2010 |

These parameters are sampled stochastically from species-specific distributions for each simulation replicate, preserving biological uncertainty.

---

## Spatial Transmission Model

### Transmission Kernel

Between-farm transmission is modeled as a distance-dependent process. The probability that farm $j$ becomes exposed during time step $t$ is:

$$P_{E_j}(t) = 1 - \prod_i \left(1 - \frac{I_i(t)}{N_i} \phi e^{-\alpha d_{ij}}\right)$$

where:

- $i$ = index of all presently infected farms
- $j$ = target (uninfected) farm
- $I_i(t)$ = number of infectious animals at farm $i$ at time $t$
- $N_i$ = total population at farm $i$
- $d_{ij}$ = Euclidean distance between farm $i$ and $j$ (kilometers)
- $\phi$ = baseline transmission probability at zero distance = **0.044**
- $\alpha$ = kernel decay parameter = **0.6** (determines steepness of distance decay)
- Maximum distance cutoff: **40 km** (beyond which transmission considered negligible)

### Kernel Parameterization

The parameters $\phi$ and $\alpha$ are derived from:

- **Livestock disease epidemiology literature**: Documented patterns of FMD spread via environmental transmission and animal movements
- **Brazilian FMD outbreak data**: Spatial analysis of 2000–2001 Rio Grande do Sul epidemics
- **International validation**: Consistent with spatial transmission estimates from European and African FMD outbreaks

### Interpretation

The kernel captures that:

1. **Local transmission dominates**: ~80–90% of between-farm spread occurs within 10 km
2. **Distance remains protective**: Risk drops 50% by ~3 km, 90% by ~12 km
3. **Rare long-distance events**: Elevated risk beyond 20 km reflects occasional long-distance movement or environmental transmission

---

## Stochastic Simulation Framework

MHASpread operates as a discrete-time stochastic simulation. At each time step $t$ (1 day):

### Step 1: Within-Farm Transmission

For each farm and each $(S, I)$ species pair:

$$\text{New exposures}_i \sim \text{Binomial}\left(S_i, \frac{I_i \beta_i}{N_i}\right)$$

### Step 2: Disease Progression

- Sampled transitions: $E \to I$, $I \to R$ from stochastic distributions of $\sigma$ and $\gamma$
- Demographic events: Births and deaths applied per farm

### Step 3: Spatial Transmission

For each uninfected farm:

$$P(\text{exposure to infection}) = P_{E_j}(t)$$

If infected, farm transitions from susceptible to exposed state.

### Step 4: Control Actions

- **Detection**: Stochastic inspection and diagnostic testing
- **Depopulation**: Removal of detected infected farms
- **Vaccination**: Progressive immunization of animals in control zones
- **Movement restrictions**: Prevention of outgoing/incoming shipments

### Output

Each simulation produces:

- Daily time series of $S, E, I, R, V$ for each farm and species
- Spatial attack patterns (which farms attacked)
- Control action outcomes (farms depopulated, animals vaccinated)
- Epidemic metrics (final size, duration, peak prevalence)

---

## Integration with Animal Movements

### Trade Network Transmission

Animal movements between farms are a critical transmission pathway. The model integrates an events file specifying:

- **Date**: When animals were moved
- **Sender, Receiver**: Movement endpoints
- **Species, Count**: Number and type of animals

If an infectious animal is moved from an infected to a susceptible farm, transmission occurs with probability dependent on:

- **Number of infectious animals** in shipment
- **Farm-level infection prevalence** at origin
- **Species susceptibility** at destination

### Standstill Implementation

During control zone activation, a 30-day movement standstill restricts:

- **Outgoing movements**: From infected + buffer zones (prevents propagation)
- **Incoming movements**: To surveillance zone (reduces exposure)

Once movement is blocked, that farm cannot receive animals for 30 days or until all control zones are dissolved.

---

## Assumptions and Simplifications

### Key Assumptions

1. **Homogeneous within-farm mixing**: Contact rates uniform across species and farms
2. **Panmictic animal populations**: No structural subdivision within farms (silos, barns treated as single unit)
3. **Constant transmission parameters**: $\beta$ and $\sigma$, $\gamma$ do not vary with season or epidemic stage
4. **Maximum distance cutoff**: Transmission negligible >40 km
5. **Instant control action execution**: Assumes perfect compliance and logistics (realistic with scenario sensitivity)
6. **Single-serotype pathogen**: Model does not represent multi-strain dynamics

### Where These Matter

- **Within-farm heterogeneity**: Species may be physically separated (reduce transmission)
- **Seasonality**: FMD temporal patterns (winter peaks in some regions)
- **Compliance variability**: Real-world standstills, vaccination uptake <100%
- **Coinfection/cross-immunity**: Other pathogens not modeled

---

## Deterministic vs. Stochastic Behavior

### Stochastic Elements

- Transmission (binomial sampling)
- Disease progression timing (distributions, not fixed)
- Detection (hypergeometric + binomial processes)
- Animal movements (probabilistic routing, if applicable)

### Deterministic Elements

- Control zone radii (fixed: 3, 7, 15 km)
- Disease duration means (fixed but distributions sampled)
- Kernel function form (exponential, fixed decay)
- Computational operations (arithmetic)

**Implication**: Replicate simulations with identical inputs will produce *variable* but statistically consistent outcomes. Ensemble analysis (100s of replicates) is standard practice.

---

## Mathematical Notation Summary

| Symbol | Meaning |
|---|---|
| $S, E, I, R, V$ | Compartment sizes (animals) |
| $N$ | Total population |
| $\beta$ | Transmission coefficient (per animal per day) |
| $\sigma$ | 1/latent period (days$^{-1}$) |
| $\gamma$ | 1/infectious period (days$^{-1}$) |
| $u$ | Birth rate |
| $v$ | Death rate |
| $d_{ij}$ | Distance between farm $i$ and $j$ (km) |
| $\phi$ | Baseline transmission probability ($d=0$) |
| $\alpha$ | Kernel decay rate (km$^{-1}$) |
| $P_E$ | Probability of farm exposure |
| $t$ | Time (days) |

---

## Next Steps

- For detailed transmission processes, see **[Transmission Dynamics](transmission_dynamics.md)**
- For control strategy implementation, see **[Control Strategies](control_strategies.md)**
- For data preparation, see **[Data Requirements](data_requirements.md)**

