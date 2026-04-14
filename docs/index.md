---
layout: default
title: Home
nav_order: 1
---

# MHASpread Documentation

**MHASpread** is a stochastic multiscale transmission model for simulating foot-and-mouth disease (FMD) and other multi-host pathogens across livestock farming networks.

---

## Documentation Map

**Technical Documentation**

- **[Model Overview](model_overview.md)** — Model structure, compartments, and mathematical foundations
- **[Transmission Dynamics](transmission_dynamics.md)** — Within-farm dynamics, spatial kernel, species parameters
- **[Control Strategies](control_strategies.md)** — Depopulation, vaccination, movement restrictions, contact tracing
- **[Economic Impact Framework](economic_impact.md)** — Integration of epidemiological and economic modeling
- **[Data Requirements](data_requirements.md)** — Input specifications and file formats

**Learning Vignettes**

- **[Introduction](vignettes/introduction.md)** — Getting started with MHASpread concepts
- **[Model Structure](vignettes/model_structure.md)** — Detailed compartmental walkthrough
- **[Transmission Processes](vignettes/transmission_processes.md)** — How infection spreads within and between farms
- **[Control Strategies](vignettes/control_strategies.md)** — Applying interventions effectively
- **[Simulation Outputs](vignettes/simulation_outputs.md)** — Interpreting model results
- **[Case Studies](vignettes/case_studies.md)** — Real-world applications and lessons learned

**Community & Training**

- **[Events and Training](events.md)** — International workshops, capacity building, and resources

---

## Quick Start

1. Start with the **[Introduction](vignettes/introduction.md)** vignette to understand core concepts.
2. Read **[Model Overview](model_overview.md)** and **[Transmission Dynamics](transmission_dynamics.md)** for the technical details.
3. Use **[Data Requirements](data_requirements.md)** and **[Model Structure](vignettes/model_structure.md)** to prepare inputs.
4. Review **[Control Strategies](control_strategies.md)** and **[Case Studies](vignettes/case_studies.md)** for applied examples.

---

## Key Concepts

| Concept | Description |
|---|---|
| **Compartmental model** | SEIR framework adapted for multiple host species with vaccination and depopulation compartments |
| **Multiscale integration** | Coupled within-farm and between-farm transmission processes |
| **Stochastic simulation** | Explicit uncertainty in transmission, detection, and control efficacy |
| **Spatial kernel** | Exponential distance-decay function for local disease spread (max 40 km) |
| **Species specificity** | Differential beta patterns reflecting biological differences (swine > cattle > small ruminants) |
| **Evidence-based parameters** | Transmission and disease duration parameters derived from peer-reviewed literature |

MHASpread explicitly models:

- **Cattle (Bovine)**: Moderate transmitters; ~3-4 day infectious period
- **Swine**: High transmitters (~5-15x more efficient than cattle); ~5-6 day infectious period
- **Small ruminants**: Low transmitters; ~3 day infectious period

Farms can contain any combination of these species with homogeneous within-farm mixing.

---

## Control Architecture

The model implements a three-zone containment strategy:

```text
Scenario Outbreak
    ↓
[INFECTED ZONE] (3 km) → Depopulation + Vaccination + Surveillance
         ↓
    [BUFFER ZONE] (7 km) → Vaccination + Surveillance
         ↓
    [SURVEILLANCE ZONE] (15 km) → Detection + Contact Tracing
```

---

## Frequently Asked Questions

### Who should use MHASpread?

- Veterinary epidemiologists conducting outbreak response research
- Policy makers evaluating control strategy effectiveness
- International organizations supporting disease surveillance
- Academic researchers studying multi-host disease dynamics
- Capacity-building institutions training epidemiologists in infectious disease modeling

### What software do I need?

MHASpread is implemented as an R package. You will need:

- **R** (version >= 4.0)
- **R packages**: Core dependencies documented in the package DESCRIPTION file
- **Basic computing resources**: Most simulations run on standard laptops

### Can I use MHASpread for diseases other than FMD?

MHASpread is parameterized for FMD but can be adapted to other multi-host pathogens. Key adaptations needed:

- Species-specific transmission coefficients (beta)
- Disease duration parameters (latent and infectious periods)
- Control strategy applicability
- Spatial transmission kernel shape

Contact the developers for guidance on adaptation.

### How do I prepare my data?

Detailed data preparation instructions are in **[Data Requirements](data_requirements.md)**. In brief:

1. Population file: Farm-level host species counts and GPS coordinates
2. Movements file: Animal shipments between farms (date, sender, receiver, species, count)
3. Control scenario file: Depopulation and vaccination capacity, detection parameters

---

## Citation

If you use materials from this documentation, please cite:

> Cardenas, N. C., Lopes, F. P. N., Machado, A., Maran, V., Trois, C., Machado, F. A., & Machado, G. (2024). Modeling foot-and-mouth disease dissemination in Rio Grande do Sul, Brazil and evaluating the effectiveness of control measures. *Frontiers in Veterinary Science*, 11, 1468864. https://doi.org/10.3389/fvets.2024.1468864

---

## License

This documentation is provided as educational material. Code and software implementations are subject to separate licensing terms. See the LICENSE file in the repository for details.

---

## Contact and Support

For questions about MHASpread documentation and methodologies:

**Machado Lab** — NC State University  
[https://machado-lab.github.io/](https://machado-lab.github.io/)

For technical support with model implementation:

**Nicolas Cespedes Cardenas** — [ORCID](https://orcid.org/0000-0001-7884-2353)
