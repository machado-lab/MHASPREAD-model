---
layout: home
title: Home
nav_exclude: true
description: "MHASpread: A Stochastic Multiscale Model for Animal Disease Spread and Control"
permalink: /
---

# MHASpread
{: .fw-700 }

**A Stochastic Multiscale Model for Animal Disease Spread and Control**
{: .fs-5 .text-grey-dk-000 }

MHASpread is an R-based simulation framework for modeling foot-and-mouth disease (FMD) — and other multi-host pathogens — across real-world livestock farming networks. Combining within-farm SEIR dynamics with spatial transmission kernels and animal-movement networks, it enables rigorous evaluation of outbreak response strategies under realistic uncertainty.

[Explore the Model](docs/model){ .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View Publications](docs/publications){ .btn .fs-5 .mb-4 .mb-md-0 }

---

## Why MHASpread?

Foot-and-mouth disease outbreaks are economically devastating, politically sensitive, and epidemiologically complex. Effective control requires understanding how the disease moves through diverse host species and heterogeneous farm networks — and how interventions ripple across those networks over time.

MHASpread was designed to answer questions that deterministic or single-species models cannot:

- How does a single infected swine shipment cascade into a regional outbreak?
- What is the marginal benefit of faster depopulation versus wider vaccination rings?
- How does outbreak size vary across 1,000 stochastic replicates under identical initial conditions?
- What is the cost-effectiveness of each control strategy under parameter uncertainty?

---

## Model at a Glance

| Feature | Description |
|---|---|
| **Spatial scales** | Within-farm SEIR + metapopulation kernel (up to 40 km) |
| **Host species** | Cattle, swine, small ruminants — each with species-specific parameters |
| **Transmission** | Mass-action within farm; exponential kernel between farms; animal movement network |
| **Control actions** | Depopulation, ring vaccination, movement standstill, contact tracing |
| **Stochasticity** | Binomial transmission sampling; PERT-distributed parameters; hypergeometric detection |
| **Economic layer** | Direct costs (depopulation, vaccination) + indirect costs (market disruption) |
| **Outputs** | Epidemic curves, farm-level attack maps, cost-effectiveness ratios |

---

## Core Transmission Kernel

Between-farm transmission is governed by an exponential spatial decay:

$$P_{E_j}(t) = 1 - \prod_{i \in \mathcal{I}(t)} \left(1 - \frac{I_i(t)}{N_i}\, \phi\, e^{-\alpha\, d_{ij}}\right)$$

where $\phi = 0.044$ is the baseline transmission probability, $\alpha = 0.6 \text{ km}^{-1}$ is the decay rate, and $d_{ij}$ is the Euclidean distance between farms $i$ and $j$.

---

## Documented Applications

| Study | Region | Focus | Reference |
|---|---|---|---|
| FMD control strategies | Brazil (Rio Grande do Sul) | Depopulation vs. vaccination trade-offs | Cespedes Cardenas & Machado (2024) |
| Epidemiological–economic integration | Brazil | Cost-effectiveness of control portfolios | Cardenas et al. (2025) |
| Strategic preparedness | Bolivia | Simulation-based vaccination requirements | Cespedes & Castillo (2023) |
| Capacity building | Chile, PAHO region | Hands-on training for national authorities | Workshops 2023–2024 |

---

## Navigate the Documentation

<div class="feature-grid">
  <div class="feature-cell">
    <strong><a href="docs/model">Model</a></strong>
    Mathematical formulation: compartments, transmission kernels, stochastic simulation framework.
  </div>
  <div class="feature-cell">
    <strong><a href="docs/methods">Methods</a></strong>
    Data requirements, economic integration, parameter uncertainty.
  </div>
  <div class="feature-cell">
    <strong><a href="docs/vignettes/">Vignettes</a></strong>
    Step-by-step conceptual tutorials from introduction to case studies.
  </div>
  <div class="feature-cell">
    <strong><a href="docs/publications">Publications</a></strong>
    Peer-reviewed papers and supporting literature.
  </div>
  <div class="feature-cell">
    <strong><a href="docs/events">Events</a></strong>
    International training workshops and capacity-building initiatives.
  </div>
</div>

---

## Primary Citation

If you use MHASpread in your research, please cite:

> Cespedes Cardenas, N., & Machado, G. (2024). Modeling foot-and-mouth disease dissemination in Brazil and evaluating the effectiveness of control measures. *Frontiers in Veterinary Science*, 11, 1468864. [https://doi.org/10.3389/fvets.2024.1468864](https://doi.org/10.3389/fvets.2024.1468864)

---

## Developers

**Nicolas Cespedes Cardenas** — Machado Lab, NC State University · [ORCID](https://orcid.org/0000-0001-7884-2353)  
**LUMAC Team** — Universidade Federal de Santa Maria, Brazil

For questions: [machado-lab.github.io](https://machado-lab.github.io/)
