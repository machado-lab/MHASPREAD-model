---
layout: home
title: Home
nav_exclude: true
description: "MHASpread: A Stochastic Multiscale Model for Animal Disease Spread and Control"
permalink: /
---

<div class="hero-box">
  <h1>MHASpread</h1>
  <p class="fs-5">A Stochastic Multiscale Model for Animal Disease Spread and Control</p>
  <p>
    MHASpread is an R-based simulation framework for modeling foot-and-mouth disease (FMD) and other
    multi-host pathogens across real-world livestock networks. It couples within-farm SEIR dynamics
    with spatial transmission kernels and animal-movement networks to evaluate outbreak responses
    under realistic uncertainty.
  </p>
  <a href="docs/model" class="btn btn-primary fs-5 mb-2 mr-2">Explore the Model</a>
  <a href="docs/publications" class="btn fs-5 mb-2">View Publications</a>
</div>

## Why MHASpread

Foot-and-mouth disease outbreaks are economically devastating, politically sensitive, and
epidemiologically complex. Effective control requires understanding how the disease moves through
heterogeneous farm networks and how interventions ripple over time.

MHASpread helps answer questions that deterministic or single-species models cannot:

- How does a single infected shipment cascade into regional outbreaks?
- What is the marginal benefit of faster depopulation versus wider vaccination rings?
- How does outbreak size vary across hundreds of stochastic replicates under identical conditions?
- What is the cost-effectiveness of each control strategy under parameter uncertainty?

---

## Key Capabilities

- Multiscale disease dynamics with within-farm SEIR and between-farm spatial transmission.
- Multi-host modeling for cattle, swine, and small ruminants with species-specific parameters.
- Integrated control actions including depopulation, ring vaccination, movement standstill, and tracing.
- Stochastic simulation with uncertainty in transmission, detection, and control effectiveness.
- Economic integration for direct and indirect cost assessment.

---

## Model at a Glance

| Feature | Description |
|---|---|
| **Spatial scales** | Within-farm SEIR + metapopulation kernel (up to 40 km) |
| **Host species** | Cattle, swine, small ruminants — each with species-specific parameters |
| **Transmission** | Mass-action within farm; exponential kernel between farms; animal movement network |
| **Control actions** | Depopulation, ring vaccination, movement standstill, contact tracing |
| **Stochasticity** | Binomial transmission sampling; PERT-distributed parameters; hypergeometric detection |
| **Economic layer** | Direct costs + indirect costs (market disruption) |
| **Outputs** | Epidemic curves, farm-level attack maps, cost-effectiveness ratios |

---

## Selected Publications

- Cardenas, N. C., Lopes, F. P. N., Machado, A., Maran, V., Trois, C., Machado, F. A., & Machado, G. (2024). Modeling foot-and-mouth disease dissemination in Rio Grande do Sul, Brazil and evaluating the effectiveness of control measures. *Frontiers in Veterinary Science*. [https://doi.org/10.3389/fvets.2024.1468864](https://doi.org/10.3389/fvets.2024.1468864)
- Cardenas, N. C., et al. (2025). Integrating epidemiological and economic models to estimate the cost of simulated foot-and-mouth disease outbreaks in Brazil. *Preventive Veterinary Medicine*. [https://doi.org/10.1016/j.prevetmed.2025.106558](https://doi.org/10.1016/j.prevetmed.2025.106558)
- Cardenas, N. C., et al. (2025). Foot-and-mouth disease in Bolivia: Simulation-based assessment of control strategies and vaccination requirements. *Transboundary and Emerging Diseases*. [https://doi.org/10.1155/tbed/9055612](https://doi.org/10.1155/tbed/9055612)

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

> Cardenas, N. C., Lopes, F. P. N., Machado, A., Maran, V., Trois, C., Machado, F. A., & Machado, G. (2024). Modeling foot-and-mouth disease dissemination in Rio Grande do Sul, Brazil and evaluating the effectiveness of control measures. *Frontiers in Veterinary Science*, 11, 1468864. https://doi.org/10.3389/fvets.2024.1468864

---

## Developers

**Nicolas Cespedes Cardenas** — Machado Lab, NC State University · [ORCID](https://orcid.org/0000-0001-7884-2353)  
**LUMAC Team** — Universidade Federal de Santa Maria, Brazil

For questions: [machado-lab.github.io](https://machado-lab.github.io/)
