---
layout: default
title: Control Strategies Guide
parent: Vignettes & Tutorials
nav_order: 4
---

# Control Strategies: Making Intervention Decisions

This vignette explains how different control strategies work in MHASpread, their trade-offs, and how to choose among them.

---

## Overview

Authorities have limited resources. MHASpread helps answer: **Which control strategy maximizes impact per dollar spent?**

---

## The Four Control Levers

### Lever 1: Depopulation Capacity

**How much**: Number of farms culled per day (1, 3, 5, 10?)

**Mechanism**: Removes all infectious animals from detected farms.

#### Trade-offs

| Capacity | Cost | Speed | Outcome |
|---|---|---|---|
| **1 farm/day** | €500k | Slow | Outbreak ~100 farms |
| **3 farms/day** | €650k | Moderate | Outbreak ~50 farms |
| **5 farms/day** | €800k | Fast | Outbreak ~20 farms |
| **10 farms/day** | €1.2M | Very fast | Outbreak ~10 farms |

**Diminishing returns**: Going from 1→5 farms/day saves ~80 farms. Going from 5→10 saves only ~10 more.

#### Decision Point

**For cattle-only regions**: Depopulation is the primary tool. Invest to capacity ≥ 3–5 farms/day.

**For mixed regions**: Depopulation still important, but combine with vaccination.

---

### Lever 2: Vaccination Timing & Capacity

**How much**: Number of farms vaccinated per day (2, 5, 10?)  
**When**: Delay from zone establishment (10, 15, 20 days?)  
**Efficacy**: Probability vaccinated animal immune (85%, 95%, 99%?)

#### Timing Tradeoff

| Lag (days) | Rationale | Cost | Effectiveness |
|---|---|---|---|
| **7 days** | Aggressive mobilization | ↑ High | ~80% of cattle protected |
| **15 days** | Standard logistics | ↓ Moderate | ~60% protected |
| **20 days** | Slow response | ↓ Low | ~40% protected |

**Reason**: Each week delay means more farms become infected; fewer cattle remain susceptible.

#### Capacity Tradeoff

| Capacity | Daily Progress | Vaccination Duration |
|---|---|---|
| **2 farms/day** | Slow (~50 days for 100 farms) | Many farms skip vaccination |
| **5 farms/day** | Moderate (~20 days for 100) | Most farms reached |
| **10 farms/day** | Fast (~10 days for 100) | All farms vaccinated quickly |

**Impact**: Faster capacity = higher coverage = better containment.

#### When to Vaccinate Over Depopulate

**Vaccination preferred if**:
- Cattle production important (economic value)
- Swine in region (vaccination of cattle prevents cross-species infection)
- Regional food security (maintain herd)

**Depopulation preferred if**:
- Speed critical (depopulation faster than vaccination)
- Cattle production already compromised (market lost)
- Limited vaccine stocks

---

### Lever 3: Movement Standstill Duration

**Fixed component**: 30 days (protocol-mandated in most countries)

**Can vary**:
- **Early termination**: If outbreak contained, lift earlier
- **Extended**: If new cases detected, extend further

#### Impact on Spread

| Standstill | Effect |
|---|---|
| **No standstill** | Network-driven cascades; 50–100% more farms infected |
| **14-day standstill** | Partial blocking; 20–30% reduction |
| **30-day standstill** | Standard; 30–50% reduction |
| **Indefinite (until clear)** | Maximum network blocking |

**Tradeoff**: Economic cost (farmers lose revenue) vs. epidemic benefit. Most countries use 30-day as compromise.

---

### Lever 4: Surveillance Intensity

**How frequent**: Daily? Weekly? Every 3 days?

**Resources**: Money spent on farm visits, testing.

#### Detection Impact

| Inspection Frequency | Detection Lag | Outbreak Size |
|---|---|---|
| **Low** (1 farm/week) | 1–2 weeks late | ~150 farms |
| **Moderate** (1/3 farms/day) | ~3 days | ~60 farms |
| **High** (1 farm/day) | ~1 day | ~30 farms |

**Finding**: Detection speed dominates outbreak size.

---

## Strategy Profiles: Putting It Together

### Strategy 1: Conservative ("Hold the Line")

**Context**: Resource-limited, border protection focus

**Parameters**:
- Depopulation: 1 farm/day
- Vaccination: 2 farms/day, 20-day lag, 90% efficacy
- Standstill: 30 days
- Surveillance: Passive only

**Outcome**: ~100–150 farms infected

**Cost**: €300–400k

**Suitable for**: Post-entry containment in small regions

---

### Strategy 2: Moderate ("Standard Protocol")

**Context**: Typical FMD preparedness plan

**Parameters**:
- Depopulation: 3–5 farms/day
- Vaccination: 5 farms/day, 15-day lag, 95% efficacy
- Standstill: 30 days
- Surveillance: Active (1/3 farms/day)

**Outcome**: ~40–80 farms infected

**Cost**: €500–700k

**Suitable for**: Large regions with moderate farming density

---

### Strategy 3: Aggressive ("Maximum Control")

**Context**: High-value region, strong political support

**Parameters**:
- Depopulation: 10 farms/day
- Vaccination: 10 farms/day, 10-day lag, 98% efficacy
- Standstill: 30 days extended if needed
- Surveillance: Active + incentivized reporting

**Outcome**: ~10–30 farms infected

**Cost**: €1–1.5M

**Suitable for**: FMD-free countries protecting export status

---

### Strategy 4: Hybrid ("Depop-Vax")

**Context**: Mixed farming systems (cattle + swine)

**Parameters**:
- Depopulation: 5 farms/day in infected zone (swine priority)
- Vaccination: 10 farms/day cattle in buffer zone, 12-day lag
- Standstill: 30 days
- Surveillance: Active (higher priority near swine farms)

**Outcome**: ~30–60 farms infected

**Cost**: €800k

**Suitable for**: Regions with both species; maximize herd preservation

---

## Decision Tree: Choosing a Strategy

```
                        START
                         ↓
         What is the primary concern?
         /        |         \
    Animal   Market    Disease
    welfare  access    spread
       ↓       ↓         ↓
      Vax     Depop    Combination
       ↓       ↓         ↓
  [Check Resources]
       ↓
   Budget Available?
   /              \
Yes (High)      No (Low)
  ↓               ↓
Aggressive    Conservative
Strategy      Strategy
```

---

## Cost-Effectiveness Comparison

### Table: Strategy vs. Outcome vs. Cost

| Strategy | Avg Farms | Median Cost | Cost/Farm | Effectiveness |
|---|---|---|---|---|
| Conservative | 120 | €350k | €2,900 | Low (baseline) |
| Moderate | 60 | €600k | €10,000 | **High** |
| Aggressive | 20 | €1.2M | €60,000 | Very high (low ROI) |
| Hybrid | 45 | €800k | €17,800 | **Highest ROI** |

**Insight**: Moderate and Hybrid strategies offer best cost-effectiveness.

---

## Real-World Constraints

### Depopulation Bottleneck

**Constraint**: Lack of trained slaughter personnel.

**Workaround**:
- Pre-identify regional facilities
- Train backup workforce
- Contract with private slaughterhouses
- Realistic capacity: 3–5 farms/day in rural areas, 10+ in urban areas

### Vaccination Bottleneck

**Constraint**: Vaccine availability.

**Workaround**:
- Pre-agreement with vaccine manufacturers for emergency supply
- Stockpile existing vaccine (monthly refresh)
- Regional equity agreements (access to regional stockpiles)
- Realistic availability: 10,000–100,000 doses per month

### Standstill Enforcement

**Constraint**: Black market movements, resistance from farmers.

**Workaround**:
- Community education on disease risk
- Tax incentives for compliance
- Enforcement via livestock inspectors at checkpoints
- Realistic enforcement: 80–95% compliance (not 100%)

---

## Scenario Testing: Sensitivity Analysis

### Question: "What if vaccine efficacy is lower than expected?"

| Efficacy | Depop 5/day | Depop 5/day + Vax |
|---|---|---|
| **95%** (baseline) | ~20 farms | ~35 farms |
| **85%** (reduced) | ~20 farms | ~50 farms |
| **75%** (poor) | ~20 farms | ~70 farms |

**Finding**: Lower vaccination efficacy shifts strategy toward depopulation reliance.

### Question: "What if we cannot achieve 5 farms/day depopulation?"

| Depopulation | Farms at Risk | Cost |
|---|---|---|
| 5/day (expected) | ~35 farms | €600k |
| 3/day (realistic) | ~60 farms | €450k |
| 1/day (worst case) | ~150 farms | €250k |

**Finding**: Depopulation capacity more important than initially budgeted.

---

## Dynamic Adaptation: Adjusting Mid-Outbreak

### Decision Point: Day 30

**Scenario**: Controls in place, but new cases still appearing.

**Options**:

1. **Continue current strategy**: "Give it time"
2. **Intensify**: "Increase depopulation/vaccination"
3. **Shift strategy**: "Switch from Moderate to Aggressive"

**Data to assess**:
- Trend in new infections: increasing or decreasing?
- Ratio of detected:total farms: adequate surveillance?
- Outbreak trajectory: on track for containment?

### Adaptive Example

**Day 30 assessment**: New cases still appearing in surveillance zone (new farms entering I compartment weekly).

**Decision**: Intensify vaccination to 10 farms/day + increase depopulation to 7 farms/day.

**Result**: Turn outbreak around by Day 45.

---

## Multi-Region Coordination

### Challenge: Outbreaks in Multiple Countries

**Scenario**: FMD detected in Argentina AND Chile simultaneously.

**Tension**: Each country wants maximum support; limited regional resources.

**MHASpread solution**: Run scenarios for different resource allocation:

- **Scenario A**: 80% resources to Argentina, 20% to Chile
- **Scenario B**: 50% to each country
- **Scenario C**: 20% to Argentina (already large), 80% to Chile (prevent establishment)

**Output**: Recommend allocation minimizing total regional impact.

---

## Policy Communication: Explaining Trade-offs

### To Government Ministers

"A moderate strategy with 3–5 farm/day depopulation + 5 farm/day vaccination will cost €600k but prevent 60+ farm outbreaks. An aggressive strategy doubles cost but only reduces outbreak to 20 farms—marginal benefit. **Recommendation: Moderate strategy is prudent.** "

### To Affected Farmers

"We are culling infected farms to protect your animals. Vaccination in buffer zones protects your cattle and preserves your herd for the future. 30-day movement restriction is temporary; we lift it once FMD cleared."

### To International Trade Partners

"Our control strategy (depopulation + ring vaccination) is aligned with OIE recommendations. Outbreak contained to <50 farm region. Trade restrictions necessary for 90 days; expect resumption by Month 4."

---

## Next Steps

- For detailed control mechanism explanation, see [Control Strategies (Technical)](../docs/control_strategies.md)
- For economic integration, see [Economic Impact Framework](../docs/economic_impact.md)
- For real-world case study, see [Case Studies Vignette](case_studies.md)

