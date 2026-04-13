[← Return to Documentation](index.md)

---

# Control Strategies

This page describes the disease control interventions implemented in MHASpread, their mechanisms, parameters, and interaction effects.

---

## Overview of Control Actions

MHASpread implements four complementary control strategies applied across three nested spatial zones. Control actions are triggered upon detection of infected farms and operate simultaneously to contain and eliminate infection.

---

## Control Zone Architecture

### Zone Definitions

Three concentric zones with increasing distance from detected infected farms:

| Zone | Radius | Primary Purpose | Key Actions |
|---|---|---|---|
| **Infected Zone** | 3 km | Rapid containment | Depopulation + vaccination + surveillance |
| **Buffer Zone** | 3–7 km | Prevent spread | Vaccination + surveillance + trade restrictions |
| **Surveillance Zone** | 7–15 km | Early detection | Active detection + movement monitoring |

### Zone Establishment

Zones are established around **each newly detected infected farm**, creating overlapping containment regions in multi-farm outbreaks.

---

## Control Action 1: Depopulation

### Mechanism

Infected farms within the infected zone are completely depopulated (all animals culled) and excluded from subsequent simulation.

### Implementation

#### Priority Ordering

Farms are prioritized for depopulation by:

1. **Primary criterion**: Total herd size (largest farms first)
2. **Rationale**: Maximize removal of infectious animals per day within capacity limits

#### Daily Capacity

The number of farms depopulated per day is a scenario parameter (e.g., 1, 3, 5 farms/day).

#### Scheduling

If depopulation capacity is exceeded:

- Farms scheduled for subsequent days maintain current dynamics
- Queued farms remain infectious until culling
- Creates realistic simulation of logistical bottlenecks

### Transmission Impact

By removing $I_i(t)$ (all infectious animals), depopulation eliminates:

- **Within-farm transmission**: No more internal spread
- **Between-farm kernel contributions**: Farm $i$ no longer contributes to $P_E$ for neighbors
- **Trade network transmission**: Depopulated farms cannot ship animals

### Disease Duration

Time from detection to removal varies. If a farm takes 3 days to depopulate and infectious period is ~5 days, ~40% of infectious life occurs pre-removal.

---

## Control Action 2: Emergency Vaccination

### Eligibility

**Vaccinated populations**: Bovine (cattle) only

**Geographic scope**: Infected zone + Buffer zone

**Timing**: 15 days after control zone establishment

### Rationale for 15-Day Lag

1. **Logistical mobilization**: Time to transport vaccine, train personnel, establish vaccination sites
2. **Epidemiological window**: Balance between early action and realistic implementation

### Implementation Details

#### Daily Vaccination Capacity

Specified as **number of farms vaccinated per day** (e.g., 5 farms/day).

#### Prioritization Within Zones

Farms prioritized by:

1. **Largest herds first** (maximize animals vaccinated)
2. **Infected zone first** (higher risk)
3. **Geographic proximity** (reduce travel time)

#### Progressive Immunization

Within each farm, vaccination occurs over multiple days:

$$a_t = \left\lfloor (S + E + I + R) \cdot \frac{1}{v_t} \right\rfloor$$

where $v_t = 15$ days (duration to vaccinate entire herd).

The number of animals attempted for vaccination per day is:

$$a_t = \min\left(a_t, \max(V^* - V, 0)\right)$$

where $V^* = \lfloor N \cdot \tau \rfloor$ is target vaccinated count and $\tau$ is target coverage (100%).

#### Vaccine Efficacy

Probability that a vaccinated animal develops protective immunity is $v_e$ (e.g., 0.95 for 95% efficacy).

Number of successful vaccinations:

$$X \sim \text{Binomial}(a_t, v_e)$$

#### Compartment Allocation

Successfully vaccinated animals are drawn proportionally from $S, E, I, R$ compartments:

$$d_k \sim \text{Hypergeometric}(C_k, U - C_k, r_k)$$

where $C_k$ is count in compartment $k$, $U$ is unvaccinated pool, and $r_k$ is remaining vaccinations to allocate.

### Mechanism: Vaccine Protection

Vaccinated animals ($V$ compartment) are:

- **Protected from infection**: Cannot transition $S \to E$ (immune-blocked)
- **Never infectious**: Do not contribute to transmission kernel
- **Not depopulated**: Remain on farm after outbreak resolution

### Population Immunity

As vaccination progresses, farm-level infection probability decreases:

$$\text{Effective susceptible proportion} = \frac{S + E}{N} \to \frac{S + E}{N - V}$$

---

## Control Action 3: Movement Standstill

### Scope

**Duration**: 30 days from zone establishment

**Geographic scope**: Infected zone + Buffer zone + Surveillance zone

### Movement Restrictions

#### Outgoing Movements

Prohibited **from** infected and buffer zones:

- Prevents infected animals from being shipped to distant farms
- Cuts network-mediated transmission pathway

#### Incoming Movements

Prohibited **to** surveillance zone:

- Reduces exposure of uninfected farms in high-surveillance area
- Limits trade-driven introduction of infection

### Implementation

Each movement event is checked against:

1. **Zone status** of sending farm
2. **Standstill date range** for that zone
3. **Movement direction** (outgoing vs. incoming)

If conditions met, movement is blocked.

### Interaction with Depopulation

Depopulated farms automatically cannot ship animals (they no longer exist in simulation).

### Duration and Termination

Standstill lasts 30 days unless:

- All zones are dissolved (rare, requires complete elimination)
- Scenario-specific override (e.g., early termination in sensitivity analysis)

---

## Control Action 4: Contact Tracing & Traceback

### Mechanism

Epidemiological tracing identifies farms that received animals from detected infected farms within the past 30 days.

### Implementation

For each detected farm $i$:

1. **Query movement history**: Identify incoming movements to farm $i$ in past 30 days
2. **Identify senders**: Origin farms of those movements
3. **Classify traceback farms**: Senders are added to **detected farms** list
4. **Apply zones**: Control zones established around traceback farms

### Transmission Impact

Traceback farms are treated as:

- **Already infected** (even if still latent)
- **Immediately under surveillance** (if in surveillance zone)
- **Eligible for depopulation** (if detection confirmed)

### Limitation

Model does not include **false positives** from traceback. All traceback identifies real infection (reflects best-case scenario).

---

## Detection Capability

### Active Surveillance

Farms in surveillance zones are inspected on a rolling basis:

#### Inspection Intensity

Number of farms inspected per day:

$$k_i = \max\left(1, \left\lfloor \frac{P_i}{3} \right\rfloor\right)$$

where $P_i$ = farms under surveillance at iteration $i$.

This ensures:

- **At least 1 farm** inspected daily if surveillance active
- **One-third of population** examined per day across full cycle

#### Detection Process

Newly infected farms detected via **hypergeometric sampling**:

$$E_i \sim \text{Hypergeometric}(M = P_i, n = I_i, N = k_i)$$

where:

- $M$ = total farms under surveillance
- $n$ = infected farms present
- $N$ = farms inspected

#### Diagnostic Sensitivity

Detected farms are subject to diagnostic testing with sensitivity $s$:

$$E_i^* \sim \text{Binomial}(E_i, s)$$

If $s = 1$: perfect diagnostics; $s < 1$: imperfect tests (reflects subclinical cases, sampling error).

---

## Scenario Parameters: A Summary

Key control parameters that users can specify:

| Parameter | Example Values | Impact |
|---|---|---|
| Depopulation capacity (farms/day) | 1, 3, 5, 10 | Speed of infected farm removal |
| Vaccination capacity (farms/day) | 2, 5, 10 | Cattle herd protection rate |
| Vaccination efficacy | 0.85, 0.95 | Proportion animals protected |
| Diagnostic sensitivity | 0.80, 0.95, 1.0 | Detection accuracy |
| Standstill duration | Fixed 30 days | Network-mediated transmission blockade |
| Detection lag | Varies with inspection | Time before control action |

---

## Control Effectiveness: Conceptual Framework

### What Drives Success?

Successful containment requires:

1. **Rapid detection** (low diagnostic lag)
2. **Adequate depopulation capacity** (removes infection before spread)
3. **Ring vaccination** (protects susceptible cattle before exposure)
4. **Movement restrictions** (blocks long-distance spread)
5. **Coordination** (zones align with actual farming landscape)

### What Undermines Control?

- **Slow detection** (infection establishes widely before awareness)
- **Low depopulation capacity** (queued farms remain sources)
- **Vaccination delays** (animals become infected during lag)
- **Movement rule violations** (non-compliance or smuggling)
- **Network clustering** (farms in tight spatial clusters, rapid spread)

### Sensitivity Considerations

MHASpread allows systematic variation of all parameters to assess robustness of strategies across different outbreak contexts.

---

## Multi-Farm Outbreaks: Zone Interaction

When multiple farms are detected simultaneously or sequentially:

1. **Zones established around each farm** (3, 7, 15 km radius)
2. **Zones may overlap** (especially in dense farming regions)
3. **Most stringent rule applied** (intersection takes minimum control intensity)

**Example**: Two farms detected 5 km apart:

- Farm A infected zone (3 km) overlaps Farm B buffer zone (7 km)
- Overlapping region subject to both infected zone and buffer zone rules
- Most restrictive controls apply (e.g., both require depopulation consideration)

---

## Temporal Dynamics of Control

### Phase 1: Silent Spread (Days 0–20)

- No control active
- Infection spreads unimpeded
- No detection

### Phase 2: Detection & Zone Establishment (Day ~21–25)

- First infected farm detected
- Zones established
- Depopulation and tracing initiated
- Vaccination logistics mobilized

### Phase 3: Active Control (Days 25–50+)

- Depopulation proceeds
- Vaccination ongoing
- Standstill enforced
- New infections may occur if control insufficient

### Phase 4: Resolution

- New infections cease
- Remaining infectious animals recover or are removed
- Zones dissolved after sufficient time with no new infections

---

## Cost-Effectiveness Perspective

Control actions impose costs:

- **Depopulation**: Direct loss (animal value) + labor + disposal
- **Vaccination**: Vaccine + personnel + logistics + time loss
- **Standstill**: Lost trade revenue + market disruption
- **Surveillance**: Personnel, testing, equipment

MHASpread outputs (final farm count, animal losses, epidemic duration) feed into downstream **economic models** for cost-benefit analysis.

---

## Next Steps

- For vaccination biology and parameters, see **[Transmission Dynamics](transmission_dynamics.md)**
- For economic integration, see **[Economic Impact Framework](economic_impact.md)**
- For control zone spatial details, see **[Model Overview](model_overview.md)**

