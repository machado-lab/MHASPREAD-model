# Introduction to MHASpread

Welcome! This vignette introduces the core concepts and philosophy of MHASpread without requiring technical expertise.

---

## What is MHASpread?

**MHASpread** is a computer model that simulates how foot-and-mouth disease (FMD) spreads across livestock farms and evaluates disease control strategies.

### Why Should You Care?

If you work in:

- **Veterinary epidemiology**: Understand disease dynamics and control effectiveness
- **Livestock policy**: Evaluate trade-offs between strategies (vaccination vs. culling)
- **Emergency response**: Plan preparedness for potential FMD entry
- **International trade**: Assess disease risk and surveillance requirements

Then MHASpread can help you make evidence-based decisions.

---

## The Disease & The Challenge

### What is FMD?

Foot-and-mouth disease is a:

- **Highly contagious** viral infection of cattle, swine, sheep
- **Major trade barrier**: Infected countries face export restrictions
- **Economic threat**: Single outbreak can cost millions in lost production
- **Global problem**: Endemic in Africa & Asia; sporadic in Americas

### Why is FMD Hard to Control?

1. **Multi-species spread**: Cattle ↔ Swine ↔ Sheep transmission varies
2. **Fast dynamics**: Animals become infectious within days
3. **Hidden spread**: Subclinical infection means farms don't "look sick"
4. **Network complexity**: Animal trade creates long-distance jumps
5. **Limited resources**: Depopulation, vaccination capacity finite

---

## How MHASpread Helps

### Core Questions MHASpread Answers

1. **"How big will an outbreak become?"** → Model estimates farm infections under different scenarios
2. **"How fast will it spread?"** → Spatial & network transmission rates quantified
3. **"Which control strategy works best?"** → Compare depopulation vs. vaccination vs. combinations
4. **"Is our response capacity enough?"** → Test if depopulation/vaccination speed sufficient
5. **"What does control cost?"** → Economic models integrate epidemiological outcomes

### What MHASpread Doesn't Do

- Predict *when* an outbreak will occur (stochastic timing not modeled)
- Represent multi-species competition or co-infection
- Model long-term herd dynamics (focuses on acute phase)
- Incorporate human behavior/psychology beyond protocol compliance

---

## Key Concepts (Non-Technical)

### The Compartmental Model

MHASpread tracks where every animal is in its disease journey:

```
Susceptible → Exposed → Infectious → Recovered
                ↓
            Vaccinated (stays protected)
                ↓
            Depopulated (removed from farm)
```

**At each farm, every day**: Some susceptible animals become infected; infected animals either recover or die.

### Why Multiple Host Species?

**The Problem**: Swine and cattle behave differently:

- **Swine**: Shed virus 10–100× more than cattle, confines tightly → rapid transmission
- **Cattle**: Lower shedding, often pasture-based → slower spread
- **Sheep**: Lowest transmitters → least amplification

A mixed cattle-swine farm is a "danger zone"—swine amplify infection rapidly.

### Spatial Transmission ("Invisible Spread")

Disease spreads from one farm to neighboring farms through:

1. **Environmental sources**: Wind-blown virus, contaminated equipment
2. **Fence-line contact**: Animals near boundaries
3. **Animal movements**: Trade shipments (most important long-distance pathway)

**Key insight**: Nearby farms at risk even if no direct contact.

### Control Zones

Authorities draw three circles around detected infected farms:

```
              ← 15 km (Surveillance Zone)
         ← 7 km (Buffer Zone)
    ← 3 km (Infected Zone)
       [INFECTED FARM]
```

**Infected zone**: Cull all animals on infected farms  
**Buffer zone**: Vaccinate susceptible cattle  
**Surveillance zone**: Actively monitor for new cases

---

## The Outbreak Scenario

### Timeline: A Typical MHASpread Simulation

**Day 0**: Single farm infected with FMD (index case; unknown to authorities)

**Days 1–20** ("Silent Spread"):
- Infection spreads within farm, then to neighbors via kernel
- May reach 10–50 farms before detection
- Meanwhile, animal trade continues unimpeded

**Day ~21** ("Detection"):
- Normal surveillance detects first clinical case
- Authorities establish control zones

**Days 22–30** ("Emergency Response"):
- Depopulation begins (limited capacity)
- Vaccination mobilized (15–20 day delay to organize)
- Movement standstill enforced
- Contact tracing identifies linked farms

**Days 30–60** ("Active Control"):
- Steady depopulation of infected farms
- Vaccination progresses in susceptible zones
- New infections decrease if control effective

**Day 60+** ("Resolution"):
- No new infections
- Remaining infectious animals recovered/removed
- Trade restrictions lifted
- Herd rebuild begins

---

## Decision Points: Evaluating Strategies

### Question: Should We Vaccinate or Depopulate?

**Classic trade-off**:

| Strategy | Pros | Cons |
|---|---|---|
| **Depopulation** | Fast; permanent; no disease risk | Animal welfare; expensive; market disruption |
| **Vaccination** | Preserves herds; faster recovery | Leaves vaccinated animals (vaccination needed periodically); higher disease risk |
| **Combination** | Speed + herd preservation | Most expensive |

**MHASpread finding** (Brazil case): Combination slightly more cost-effective (~€500 vs. €600 per farm protected).

### Question: How Capacity Should We Invest In?

**Resource allocation**:

- **Low capacity** (1 farm/day depopulation): Slow response, outbreaks large
- **Moderate capacity** (3–5 farms/day): Balances cost & effectiveness
- **High capacity** (10+ farms/day): Expensive but marginal improvement

**MHASpread analysis**: Diminishing returns at high capacity (investment may not justify benefit).

---

## Real-World Application Examples

### Example 1: Brazil's 2000–2001 FMD Outbreak

**Context**: Rio Grande do Sul state, mixed cattle-swine farming  
**Outcome**: 2,000+ farms affected, $100 million+ in losses

**What-if simulation with MHASpread**: 
- With faster depopulation (5 farms/day vs. actual ~1–2): Outbreak would have been 50% smaller
- Addition of emergency vaccination: Could have prevented additional 200 farms

**Lesson**: Surge capacity important; preparedness saves money.

### Example 2: Bolivia Risk Assessment (2023)

**Context**: Mixed cattle-llama system, resource-limited

**Scenario tested**: "What if 1 infected farm enters Bolivia from Argentina?"

**MHASpread result**:
- Without control: 100+ farms affected in 3 months
- With realistic capacity (2 farms/day depopulation): 15–20 farms affected
- Cost of control (~€300k) far less than uncontrolled outbreak (~€5–10 million)

**Conclusion**: Justified investment in surveillance and response infrastructure.

---

## Key Takeaways

1. **FMD spreads fast**: ~20 day "silent spread" creates large undetected outbreaks
2. **Early detection critical**: Reduces number of infected farms at control start
3. **No perfect control**: Depopulation fastest but costly; vaccination preserves herds but riskier
4. **Capacity matters**: Doubling depopulation speed can halve outbreak size
5. **Context varies**: Brazil ≠ Bolivia ≠ Chile; customized strategies needed

---

## What's Next?

Explore MHASpread in more depth:

1. **Want to understand the model?** → Read [Model Structure Vignette](model_structure.md)
2. **Ready to learn about transmission?** → [Transmission Processes Vignette](transmission_processes.md)
3. **Interested in control strategies?** → [Control Strategies Vignette](control_strategies.md)
4. **Want to see real examples?** → [Case Studies Vignette](case_studies.md)
5. **Ready to run simulations?** → Consult [example_script.md](../example_script.md) and [Data Requirements](../docs/data_requirements.md)

---

## Glossary (Quick Reference)

- **SEIR**: Susceptible-Exposed-Infectious-Recovered (disease stages)
- **β (beta)**: Transmission rate (infected-to-susceptible encounters per day)
- **Latent period**: Days between infection and becoming infectious
- **Infectious period**: Days an animal sheds virus
- **Kernel**: Mathematical function describing transmission vs. distance
- **Metapopulation**: Network of farms as interconnected units
- **Stochastic**: Incorporating randomness (outcomes vary between runs)
- **Depopulation**: Culling all animals on infected farms
- **Vaccination**: Emergency immunization of susceptible animals
- **Standstill**: Ban on animal movements
- **Zone**: Geographic region with specified control intensity

---

## Further Reading

- **Technical overview**: [Model Overview](../docs/model_overview.md)
- **Publications**: Cespedes & Machado (2024), Frontiers in Vet Science; Cardenas et al. (2024), Preventive Vet Medicine
- **Workshop materials**: [PAHO Repository](https://github.com/machado-lab/MHASpread_workshop_PAHO/)

