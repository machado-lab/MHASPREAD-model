---
layout: default
title: Model
nav_exclude: true
has_children: false
permalink: docs/model
---

# Model
{: .fw-700 }

The scientific core of MHASpread — mathematical formulation, transmission mechanisms, and control architecture.
{: .fs-5 .text-grey-dk-100 }

---

MHASpread operates at two coupled spatial scales:

1. **Within-farm scale** — stochastic SEIR dynamics for each host species in isolation
2. **Metapopulation scale** — spatial kernel and animal-movement network connecting farms

Both scales are resolved daily in a discrete-time stochastic simulation loop.

---

## Section Overview

| Page | Content |
|---|---|
| [**Model Overview**](model_overview) | Compartmental structure, differential equations, stochastic framework |
| [**Transmission Dynamics**](transmission_dynamics) | Species-specific $\beta$, spatial kernel mechanics, trade-network transmission |
| [**Control Strategies**](control_strategies) | Depopulation, ring vaccination, movement standstill, contact tracing |

---

## Mathematical Notation

The table below summarises the core symbols used throughout the Model section.

| Symbol | Meaning | Units |
|---|---|---|
| $S, E, I, R, V$ | Compartment sizes (susceptible, exposed, infectious, recovered, vaccinated) | animals |
| $N_i$ | Total population at farm $i$ | animals |
| $\beta$ | Transmission coefficient | animal$^{-1}$ day$^{-1}$ |
| $\sigma$ | Mean latent period | days |
| $\gamma$ | Mean infectious period | days |
| $\phi$ | Baseline between-farm transmission probability | dimensionless |
| $\alpha$ | Spatial kernel decay rate | km$^{-1}$ |
| $d_{ij}$ | Euclidean distance between farm $i$ and $j$ | km |
| $P_{E_j}(t)$ | Probability that farm $j$ is exposed at time $t$ | dimensionless |

---

*Continue to [Model Overview](model_overview) →*
