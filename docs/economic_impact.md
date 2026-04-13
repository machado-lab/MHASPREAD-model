[← Return to Documentation](index.md)

---

# Economic Impact Framework

MHASpread integrates epidemiological simulation with economic modeling to assess the cost-effectiveness of disease control strategies.

---

## Overview

While MHASpread provides detailed **epidemiological outputs** (infected farms, animals affected, outbreak duration), downstream economic models incorporate these results to evaluate:

- **Direct costs**: Depopulation, vaccination, diagnosis
- **Indirect costs**: Market disruption, trade restrictions, lost productivity  
- **Benefits**: Disease avoidance, prevented spread
- **Cost-effectiveness**: Cost per farm or animal protected

---

## Epidemiological Outputs from MHASpread

MHASpread simulations generate outputs that feed into economic models:

### Farm-Level Outcomes

- **Farms depopulated**: Number culled as disease control
- **Farms vaccinated**: Number receiving emergency ring vaccination
- **Farms infected**: Total farms affected by disease
- **Escape farms**: Uninfected farms outside control zones

### Animal-Level Outcomes

By species and compartment:

- **Total animals infected**: Sum of $I$ compartment across all farms
- **Animals recovered**: Naturally immune survivors
- **Animals vaccinated**: Cattle receiving emergency vaccine
- **Animals depopulated**: Culled animals (by species)

### Temporal Outcomes

- **Outbreak duration**: Days from index case to elimination
- **Silent spread duration**: Undetected pre-control phase
- **Active control duration**: Days with ongoing interventions
- **Peak prevalence**: Maximum infected animals/farms simultaneously

### Network Outcomes

- **Movement events blocked**: Standstill-prevented trade
- **Traceback links traced**: Farms identified via contact tracing
- **Spatial extent**: Maximum distance from index farm of last case

---

## Cost Components

### Direct Costs

#### Depopulation Costs

$$\text{Cost}_{\text{depop}} = \sum_{\text{depopulated farms}} (\text{animal count} \times \text{value per animal}) + \text{labor} + \text{disposal}$$

- **Unit costs vary by species** (cattle >swine > sheep)
- **Example values**: Cattle €500/animal, Swine €150/animal, Sheep €50/animal
- **Labor/disposal**: Often €1000–5000 per farm
- **Total**: Typically 40–60% of outbreak economic cost

#### Vaccination Costs

$$\text{Cost}_{\text{vax}} = \text{farms vaccinated} \times (\text{vaccine} + \text{personnel} + \text{logistics})$$

- **Vaccine cost**: €1–2 per dose
- **Personnel**: Veterinarians, laborers (€10–30/hour, ~2 hours per farm)
- **Logistics**: Transport, equipment, cold storage (~€500–1000 per farm)
- **Total per farm vaccinated**: €2000–5000

#### Surveillance & Diagnosis

$$\text{Cost}_{\text{surv}} = \text{inspections} \times \text{cost per farm} + \text{diagnostic tests} \times \text{cost per test}$$

- **Farm inspection**: €50–200 per farm visit
- **Diagnostic test (serology, RT-PCR)**: €20–100 per test
- **Typically 5–15% of total cost**

#### Movement Standstill (Lost Trade)

$$\text{Cost}_{\text{standstill}} = \text{affected farms} \times \text{lost revenue per day} \times 30 \text{ days}$$

- **Lost revenue**: Variable (dairy income, market premium loss, feed cost)
- **Estimation**: Average farm revenue / 365 days × number restricted farms × 30
- **Can be substantial** (€500–2000 per farm)

### Indirect Costs

#### Market Disruption

- **Export bans**: Trade restrictions limiting market access
- **Consumer confidence loss**: Demand reduction post-outbreak
- **Restocking delays**: Time and cost to rebuild herds

#### Productivity Loss

- **Production interruption**: Farms under control have limited output
- **Herd reconstruction**: Time and cost to rebuild breeding stock
- **Genetic loss**: Valuable breeding lines culled

---

## Cost-Effectiveness Metrics

### Cost Per Farm Protected

$$\text{CER}_{\text{farm}} = \frac{\text{Total Control Cost}}{\text{Farms Prevented from Infection}}$$

**Example**: Control cost €500,000; 50 farms prevented infection = €10,000 per farm protected

### Cost Per Animal Saved

$$\text{CER}_{\text{animal}} = \frac{\text{Total Control Cost}}{\text{Animals Saved from Infection}}$$

**Example**: Control cost €500,000; 10,000 animals saved = €50 per animal

### Outbreak Cost Without Control (Counterfactual)

Estimate total cost if disease spread uncontrolled:

$$\text{Uncontrolled Cost} = \text{Infected farms} \times \text{avg farm value loss}$$

**Typical range**: €100,000–5 million depending on region

### Net Benefit

$$\text{Net Benefit} = \text{Uncontrolled Cost} - \text{Control Cost}$$

**Interpretation**: 

- **Positive**: Control is cost-effective (savings exceed cost)
- **Negative**: Control costs exceed prevented damage (rare for FMD)

---

## Scenario Comparison Framework

### Comparison of Multiple Control Strategies

Simultaneously simulate several scenarios:

| Scenario | Depop Capacity | Vax Capacity | Vax Delay | Control Cost | Farms Protected | CER |
|---|---|---|---|---|---|---|
| **Conservative** | 1 farm/day | 2 farms/day | 20 days | €300k | 40 | €7,500 |
| **Moderate** | 3 farms/day | 5 farms/day | 15 days | €450k | 52 | €8,650 |
| **Aggressive** | 5 farms/day | 10 farms/day | 10 days | €650k | 58 | €11,200 |
| **No control** | 0 | 0 | ∞ | €0 | 0 | N/A |

**Insight**: Moderate scenario often optimal (diminishing returns at aggressive levels).

---

## Integration with MHASpread Outputs

### Workflow

1. **Run MHASpread** for each scenario → generates epidemiological outputs
2. **Export results**: Farms infected, animals in each compartment, duration
3. **Assign unit costs** to animal/farm outcomes
4. **Calculate total cost** per scenario
5. **Compare across scenarios** using cost-effectiveness metrics
6. **Sensitivity analysis**: Vary cost assumptions, repeat

### R-Based Integration

```r
# After running MHASpread simulation
outbreak_results <- run_mhaspread(population, events, scenario_params)

# Extract economic inputs
depop_farms <- sum(outbreak_results$depopulation_events$count > 0)
vax_farms <- sum(outbreak_results$vaccination_events$count > 0)
infected_farm_total <- nrow(unique(outbreak_results$infected_farm_timeseries))

# Assign unit costs (€)
cost_depop <- depop_farms * 1500  # €1500 per farm depopulated
cost_vax <- vax_farms * 3000      # €3000 per farm vaccinated
cost_surveillance <- infected_farm_total * 200  # €200 per farm inspected

# Total control cost
control_cost <- cost_depop + cost_vax + cost_surveillance

# Farms protected (prevented infection)
farms_protected <- total_farms_region - infected_farm_total

# Cost-effectiveness ratio
cer <- control_cost / farms_protected
```

---

## Uncertainty & Sensitivity Analysis

### Parameter Sensitivity

Repeat cost-effectiveness analysis with varying cost assumptions:

| Cost Component | Low | Base | High | Impact |
|---|---|---|---|---|
| Depopulation per farm | €1000 | €1500 | €2500 | ±40% CER change |
| Vaccination per farm | €2000 | €3000 | €5000 | ±30% CER change |
| Surveillance per farm | €100 | €200 | €400 | ±10% CER change |

**Output**: Confidence intervals on cost-effectiveness estimates

### Probabilistic Analysis

Rather than point estimates, incorporate distributions:

- **Depopulation costs**: Lognormal(μ=€1500, σ=€300)
- **Vaccination costs**: Uniform(€2000, €5000)
- **Outcome uncertainty**: From 100+ MHASpread replicates

**Result**: Posterior distribution of cost-effectiveness across scenarios

---

## Comparative Examples

### Case Study: Brazil FMD Preparedness

**Region**: Rio Grande do Sul (~80 cattle farms, 20 swine, 15 sheep farms)  
**Outbreak Scenario**: Single farm infection  
**Uncontrolled cost**: €2.5 million (estimated from cascading losses)

| Strategy | Control Cost | Farms Protected | CER |
|---|---|---|---|
| Reactive (slow depop) | €600k | 60 | €10k per farm |
| Proactive (fast depop + vax) | €850k | 95 | €9k per farm |
| **Cost-effective threshold** | - | - | **€15k/farm** |

**Interpretation**: Proactive strategy justified (below threshold).

### Case Study: Bolivia FMD Surveillance

**Region**: Mixed cattle/llama system (resource-limited)  
**Outbreak Scenario**: Two simultaneous index farms

| Strategy | Depop Capacity | Vax Efficacy | Net Benefit | Recommended? |
|---|---|---|---|---|
| Minimal intervention | 0.5 farms/day | None | -€1.2M | No |
| Realistic capacity | 2 farms/day | 85% | +€800k | **Yes** |
| Ideal (if funded) | 5 farms/day | 95% | +€1.5M | Yes but expensive |

---

## Policy Implications

### Break-Even Analysis

What control capacity is needed to justify costs?

**Question**: At what depopulation capacity does outbreak control become cost-effective?

$$\text{Depopulation capacity} \geq \frac{\text{Uncontrolled Cost}}{\text{Cost per farm depopulated}}$$

**Example**: If uncontrolled cost €2M and depopulation costs €1500/farm:

$$\text{Required capacity} \geq 1,333 \text{ farms} = \text{Probably achievable}$$

### Preparedness Investment

Should a country invest in surge capacity (vans, vaccine stocks, trained personnel)?

**Cost-benefit**: Compare investment cost vs. expected savings across multiple outbreak scenarios.

---

## Limitations

1. **Exclusion of trade bans**: Model not designed to handle multi-country trade restrictions
2. **Longer-term impacts**: Only covers acute outbreak period (~year 1)
3. **Indirect costs underestimated**: Market disruption, consumer confidence not fully modeled
4. **Regional heterogeneity**: Cost assumptions may vary by farm type, location
5. **Uncertainty propagation**: Stochastic model uncertainty + cost uncertainty compounded

---

## References & Further Reading

Key references for cost-effectiveness analysis integration:

- Cardenas et al. (2024): [Integrating epidemiological and economic models to estimate the cost of simulated FMD outbreaks in Brazil](https://doi.org/10.1016/j.prevetmed.2025.106558)
- USDA/APHIS economic impact assessments
- FAO FMD cost-benefit manuals

---

## Next Steps

- For control strategy details, see **[Control Strategies](control_strategies.md)**
- For case study examples, see **[Case Studies Vignette](../vignettes/case_studies.md)**
- For epidemiological outputs to feed this framework, see **[Model Overview](model_overview.md)**

