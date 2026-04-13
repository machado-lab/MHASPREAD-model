---
layout: default
title: Documentation Hub
nav_exclude: true
---

# MHASpread: Documentation Hub

**MHASpread** is a stochastic multiscale transmission model for simulating foot-and-mouth disease (FMD) and other multi-host pathogens across livestock farming networks.

---

## 📚 Documentation Pages

### **Technical Documentation**
| Page | Description |
|---|---|
| **[Model Overview](model_overview.md)** | Comprehensive model structure, compartments, and mathematical foundations |
| **[Transmission Dynamics](transmission_dynamics.md)** | Within-farm dynamics, spatial transmission kernel, and species-specific parameters |
| **[Control Strategies](control_strategies.md)** | Depopulation, vaccination, movement restrictions, and contact tracing |
| **[Economic Impact Framework](economic_impact.md)** | Integration of epidemiological and economic modeling |
| **[Data Requirements](data_requirements.md)** | Input specifications, data preparation, and file formats |

### **Learning Vignettes (Tutorials)**
| Page | Description |
|---|---|
| **[Introduction](vignettes/introduction.md)** | Getting started with MHASpread concepts |
| **[Model Structure](vignettes/model_structure.md)** | Detailed compartmental model walkthrough |
| **[Transmission Processes](vignettes/transmission_processes.md)** | How infection spreads within and between farms |
| **[Control Strategies](vignettes/control_strategies.md)** | Applying interventions effectively |
| **[Simulation Outputs](vignettes/simulation_outputs.md)** | Interpreting model results |
| **[Case Studies](vignettes/case_studies.md)** | Real-world applications and lessons learned |

### **Community & Training**
| Page | Description |
|---|---|
| **[Events and Training](events.md)** | International workshops, capacity-building initiatives, and resources |

---

## Quick Navigation

---

## Key Concepts

### At a Glance

| Concept | Description |
|---|---|
| **Compartmental Model** | SEIR framework adapted for multiple host species with vaccination and depopulation compartments |
| **Multiscale Integration** | Coupled within-farm and between-farm transmission processes |
| **Stochastic Simulation** | Explicit uncertainty in transmission, detection, and control efficacy |
| **Spatial Kernel** | Exponential distance-decay function for local disease spread (max 40 km) |
| **Species Specificity** | Differential β patterns reflecting biological differences (swine >> cattle > small ruminants) |
| **Evidence-Based Parameters** | Transmission and disease duration parameters derived from peer-reviewed literature |

### Multi-Host Dynamics

MHASpread explicitly models:

- **Cattle (Bovine)**: Moderate transmitters; ~3–4 day infectious period
- **Swine**: High transmitters (~5–15× more efficient than cattle); ~5–6 day infectious period
- **Small Ruminants**: Low transmitters; ~3 day infectious period

Farms can contain any combination of these species with homogeneous within-farm mixing.

### Control Architecture

The model implements a three-zone containment strategy:

```
Scenario Outbreak
    ↓
[INFECTED ZONE] (3 km) → Depopulation + Vaccination + Surveillance
         ↓
    [BUFFER ZONE] (7 km) → Vaccination + Surveillance
         ↓
    [SURVEILLANCE ZONE] (15 km) → Detection + Contact Tracing
```

---

## Getting Started

1. **New to MHASpread?** Start with the [Introduction vignette](../vignettes/introduction.md)
2. **Want model details?** Read [Model Overview](model_overview.md) and [Transmission Dynamics](transmission_dynamics.md)
3. **Planning simulations?** Check [Data Requirements](data_requirements.md) and [Model Structure vignette](../vignettes/model_structure.md)
4. **Evaluating controls?** See [Control Strategies](control_strategies.md) and [Case Studies vignette](../vignettes/case_studies.md)

---

## Scientific Background

MHASpread development builds on three decades of epidemiological research on foot-and-mouth disease and multi-host infectious disease dynamics. The model integrates insights from:

- **Experimental transmission studies** (FMD susceptibility and shedding by species)
- **Field outbreak investigations** (spatial spread patterns and movement-mediated transmission)
- **Mathematical epidemiology** (multiscale modeling and stochastic network dynamics)
- **Control effectiveness evaluations** (empirical data on vaccination and culling)

Key references are documented in each technical section and in the main [README](../README.md).

---

## Frequently Asked Questions

### Who should use MHASpread?

- **Veterinary epidemiologists** conducting outbreak response research
- **Policy makers** evaluating control strategy effectiveness
- **International organizations** (FAO, OIE, PAHO) supporting disease surveillance
- **Academic researchers** studying multi-host disease dynamics
- **Capacity-building institutions** training epidemiologists in infectious disease modeling

### What software do I need?

MHASpread is implemented as an R package. You'll need:

- **R** (version ≥ 4.0)
- **R packages**: (Core dependencies documented in the package DESCRIPTION file)
- **Basic computing resources**: Most simulations run on standard laptops

### Can I use MHASpread for diseases other than FMD?

MHASpread is parameterized for FMD but can be adapted to other multi-host pathogens. Key adaptations needed:

- Species-specific transmission coefficients (β)
- Disease duration parameters (latent and infectious periods)
- Control strategy applicability
- Spatial transmission kernel shape

Contact the developers for guidance on adaptation.

### How do I prepare my data?

Detailed data preparation instructions are in [Data Requirements](data_requirements.md). In brief:

1. **Population file**: Farm-level host species counts, GPS coordinates (degrees and UTM)
2. **Movements file**: Animal shipments between farms (date, sender, receiver, species, count)
3. **Control scenario file**: Depopulation and vaccination capacity, detection parameters

See the example scripts in the MHASPREAD-model folder for templates.

---

## Glossary

- **β (Transmission coefficient)**: Probability of transmission per infected–susceptible contact per day, species and context-dependent
- **SEIR**: Susceptible-Exposed-Infectious-Recovered compartmental model
- **E (Exposed)**: Infected but not yet infectious (incubation period)
- **Kernel (Transmission kernel)**: Mathematical function describing transmission probability as a function of distance
- **Metapopulation**: Network of discrete farm-level populations
- **Stochastic**: Incorporating random variation; non-deterministic outcomes
- **Traceability**: Epidemiological tracking of animal movement for disease control
- **Zone (Control zone)**: Geographic region with specified intervention intensity

---

## Citation

If you use materials from this documentation, please cite:

> Cespedes Cardenas, N., & Machado, G. (2024). Modeling foot-and-mouth disease dissemination in Brazil and evaluating the effectiveness of control measures. *Frontiers in Veterinary Science*, 11, 1468864. https://doi.org/10.3389/fvets.2024.1468864

---

## License

This documentation is provided as educational material. Code and software implementations are subject to separate licensing terms. See the LICENSE file in the repository for details.

---

## Contact & Support

For questions about MHASpread documentation and methodologies:

📧 **Machado Lab** — NC State University  
🔗 [machado-lab.github.io](https://machado-lab.github.io/)

For technical support with model implementation:

📧 **Nicolas Cespedes Cardenas** — [ORCID](https://orcid.org/0000-0001-7884-2353)

