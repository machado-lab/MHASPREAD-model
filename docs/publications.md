---
layout: default
title: Publications
nav_order: 6
permalink: docs/publications
---

# Publications
{: .fw-700 }

Peer-reviewed papers describing, applying, and validating the MHASpread framework.
{: .fs-5 .text-grey-dk-100 }

---

## Primary Reference

The following paper is the **primary citation** for MHASpread. Please cite it whenever you use the model in research or training.

---

### Modeling FMD Dissemination in Brazil

**Cespedes Cardenas, N., & Machado, G.** (2024).  
Modeling foot-and-mouth disease dissemination in Brazil and evaluating the effectiveness of control measures.  
*Frontiers in Veterinary Science*, 11, 1468864.  
[https://doi.org/10.3389/fvets.2024.1468864](https://doi.org/10.3389/fvets.2024.1468864)

**Journal**: Frontiers in Veterinary Science (open access)

**Contribution to MHASpread**:  
This paper introduces the MHASpread framework and provides its primary scientific validation. Using farm-level network data from Rio Grande do Sul (2000–2001 epidemic), the study demonstrates how the stochastic multiscale model reproduces empirically observed outbreak dynamics and evaluates the comparative effectiveness of depopulation, ring vaccination, and movement standstill strategies. The spatial kernel parameterization ($\phi = 0.044$, $\alpha = 0.6$ km$^{-1}$) and species-specific transmission coefficients ($\beta$ matrix) reported here are the defaults embedded in the model.

---

## Supporting Publications

---

### Epidemiological–Economic Integration

**Cardenas, N. C., Cespedes Cardenas, N., & Machado, G.** (2025).  
Integrating epidemiological and economic models to estimate the cost of simulated foot-and-mouth disease outbreaks in Brazil.  
*Preventive Veterinary Medicine*, 106558.  
[https://doi.org/10.1016/j.prevetmed.2025.106558](https://doi.org/10.1016/j.prevetmed.2025.106558)

**Journal**: Preventive Veterinary Medicine (Elsevier / ScienceDirect)

**Contribution to MHASpread**:  
This paper extends MHASpread with an economic integration layer, enabling cost-effectiveness analysis alongside epidemiological outcomes. It introduces the framework for computing direct costs (depopulation, vaccination, active surveillance) and indirect costs (market disruption, trade restrictions, productivity losses). Cost-effectiveness ratios (CER) per farm and per animal are derived from ensemble simulation outputs, providing a decision-support tool for comparing control portfolios.

---

### Strategic Preparedness in Bolivia

**Cespedes, N., & Castillo, A.** (2023).  
Foot-and-mouth disease in Bolivia: Simulation-based assessment of control strategies and vaccination requirements.  
*Transboundary and Emerging Diseases*, 9055612.  
[https://doi.org/10.1155/tbed/9055612](https://doi.org/10.1155/tbed/9055612)

**Journal**: Transboundary and Emerging Diseases (Wiley)

**Contribution to MHASpread**:  
This study demonstrates MHASpread's applicability beyond its original Brazilian parameterization context. Applied to Bolivian livestock network data, it evaluates surveillance sensitivity requirements and vaccination ring size trade-offs under endemic-to-epidemic transition scenarios. The paper establishes the model's generalizability as a preparedness platform for countries with limited outbreak history — key for PAHO/FAO-supported capacity building in the region.

---

## Related Workshops and Technical Reports

| Event | Year | Organization | Resource |
|---|---|---|---|
| Chile FMD Simulation Modeling Workshop | 2024 | SAG Chile | [workshop_aftosa_chile_2024](https://machado-lab.github.io/workshop_aftosa_chile_2024/) |
| PANAFTOSA Regional Workshop Rio | 2023 | PAHO / PANAFTOSA | [PANAFTOSA-Workshop-Rio2023](https://machado-lab.github.io/PANAFTOSA-Workshop-Rio2023/) |
| PAHO MHASpread Workshop Repository | 2023 | PAHO | [MHASpread_workshop_PAHO](https://github.com/machado-lab/MHASpread_workshop_PAHO/) |

---

## Citation Format

**APA (7th edition)**

> Cespedes Cardenas, N., & Machado, G. (2024). Modeling foot-and-mouth disease dissemination in Brazil and evaluating the effectiveness of control measures. *Frontiers in Veterinary Science*, *11*, 1468864. https://doi.org/10.3389/fvets.2024.1468864

**BibTeX**

```bibtex
@article{cespedescardenas2024,
  author  = {Cespedes Cardenas, Nicolas and Machado, Gustavo},
  title   = {Modeling foot-and-mouth disease dissemination in Brazil and
             evaluating the effectiveness of control measures},
  journal = {Frontiers in Veterinary Science},
  year    = {2024},
  volume  = {11},
  pages   = {1468864},
  doi     = {10.3389/fvets.2024.1468864}
}
```

---

## Acknowledgments

MHASpread development is supported by **FUNDESA RS**, **NC State University** (Department of Population Health and Pathobiology), and **LUMAC** (Universidade Federal de Santa Maria).

---

[← Home](/) · [Events & Training](events)
