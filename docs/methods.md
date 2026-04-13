---
layout: default
title: Methods
nav_order: 3
has_children: true
permalink: docs/methods
---

# Methods
{: .fw-700 }

Input data specifications, modeling assumptions, and economic integration framework.
{: .fs-5 .text-grey-dk-100 }

---

The Methods section covers everything needed to parameterize, configure, and interpret MHASpread simulations — from data preparation to cost-effectiveness analysis.

---

## Section Overview

| Page | Content |
|---|---|
| [**Data Requirements**](data_requirements) | Population database, movement records, scenario parameters, file formats |
| [**Economic Impact**](economic_impact) | Cost components, cost-effectiveness metrics, scenario comparison framework |

---

## Key Inputs at a Glance

MHASpread requires three primary input files:

| File | Description | Key Fields |
|---|---|---|
| **Population database** | One row per farm; species counts and GPS coordinates | `farm_id`, `lat`, `long`, `cattle`, `swine`, `small_ruminants` |
| **Events database** | Animal shipments between farms | `date`, `sender_id`, `receiver_id`, `species`, `n_animals` |
| **Scenario parameters** | Control strategy settings for the simulation run | depopulation capacity, vaccination rate, detection sensitivity |

---

## Modeling Assumptions

Key assumptions underlying the MHASpread framework:

- **Homogeneous within-farm mixing** — contact rates uniform across all animals on a farm
- **Exponential distance decay** — transmission probability falls exponentially with distance
- **PERT-distributed parameters** — biological uncertainty captured via beta-family distributions
- **Perfect compliance** — control actions (standstill, vaccination) assumed to be fully enforced
- **Single-serotype pathogen** — no cross-immunity or multi-strain dynamics

See [Data Requirements](data_requirements) for full parameter lists and [Model Overview](model_overview) for assumption implications.

---

*Continue to [Data Requirements](data_requirements) →*
