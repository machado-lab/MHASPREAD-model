# MHASpread: A Metapopulation Framework for Animal Disease Spread and Control

![SEIR_model](https://img.shields.io/badge/SEIR_model-multilevel-blue) ![Stochastic](https://img.shields.io/badge/Stochastic-enabled-brightgreen) ![Multi-species](https://img.shields.io/badge/Multi--host-3%20species-blue) ![Spatial_Transmission](https://img.shields.io/badge/Spatial_Transmission-kernel--based-orange) ![Control_Actions](https://img.shields.io/badge/Control_Actions-implemented-success)

## Overview

**MHASpread** is a comprehensive stochastic, multiscale transmission model designed to simulate epidemic trajectories of foot-and-mouth disease (FMD) and other multi-host pathogens across livestock farming networks. The framework integrates within-farm disease dynamics, spatial transmission processes, and realistic disease control interventions to enable rigorous evaluation of outbreak response strategies.

The model operates at two coupled scales:

- **Within-farm scale**: Individual-based SEIR dynamics for multiple host species
- **Metapopulation scale**: Network-level transmission via spatial kernels and animal movements

MHASpread is particularly suited for:

- **Epidemiological research**: Understanding multi-host disease dynamics and species-specific transmission
- **Policy evaluation**: Assessing the effectiveness and cost-efficiency of control strategies
- **Emergency preparedness**: Simulating outbreak scenarios and informing contingency planning
- **International capacity building**: Supporting training and knowledge transfer in disease surveillance and control

## Scientific Background

Foot-and-mouth disease (FMD) is a highly contagious viral disease of cloven-hoofed livestock with significant economic and trade implications. FMD demonstrates complex transmission dynamics due to:

- **Multi-host nature**: Differential susceptibility and transmissibility across cattle, swine, and small ruminants
- **Environmental persistence**: Heterogeneous transmission via direct contact, aerosol, and fomites
- **Spatial heterogeneity**: Disease spread constrained by distance but facilitated by animal movements
- **Species-specific traits**: Varying latent periods, infectious periods, and recovery rates

MHASpread explicitly models these complexities to provide mechanistic insights into outbreak progression under diverse epidemiological and management scenarios.

---

## Model Description

MHASpread is built on a stochastic, SEIR-based framework with distinct compartments for each host species:

| Compartment | Definition |
|---|---|
| **S** | Susceptible: animals not infected and able to acquire infection |
| **E** | Exposed: infected but not yet infectious (latent period) |
| **I** | Infectious: infected animals capable of transmitting infection |
| **R** | Recovered: animals recovered with temporary or permanent immunity |
| **V** | Vaccinated: animals with vaccine-induced immunity |

### Within-Farm Dynamics

Disease progression within farms follows species-specific transmission probabilities (β) and disease duration parameters:

- **Transmission coefficient (β)**: Determined by both infected and susceptible species, reflecting differential transmissibility (e.g., swine are more efficient FMD spreaders than cattle)
- **Latent period (σ)**: Average 2–5 days, species-dependent
- **Infectious period (γ)**: Average 3–6 days, species-dependent
- **Vital dynamics**: Births and deaths incorporated where data available

### Spatial Transmission

Local spread between farms is modeled using an exponential transmission kernel:

$$P_E(t) = 1 - \prod_i \left(1 - \frac{I_i(t)}{N_i} \phi e^{-\alpha d_{ij}}\right)$$

where:
- $d_{ij}$ = distance between farms (maximum 40 km)
- $\phi$ = baseline transmission probability (0.044)
- $\alpha$ = kernel decay parameter (0.6)
- $I_i(t)/N_i$ = infection prevalence at farm $i$

This formulation captures the empirically observed pattern of decreasing transmission risk with distance, bounded by documented maximum dispersal distances.

### Disease Detection

Infected farms are detected through active surveillance using a hypergeometric sampling process that:

1. Inspects a fraction of farms under surveillance (≥1 farm)
2. Identifies infected farms accounting for finite surveillance population
3. Incorporates diagnostic imperfection (sensitivity $s$)
4. Tracks traceback-identified farms through contact tracing

### Control Interventions

MHASpread implements four primary control strategies:

| Control | Description | Implementation |
|---|---|---|
| **Depopulation** | Culling of infected farms | Priority by herd size; daily capacity limits |
| **Emergency Vaccination** | Ring vaccination of bovine herds | Infected + buffer zones; 15-day lag; daily capacity limits |
| **Movement Standstill** | Restriction on animal movements | 30-day duration across infected, buffer, and surveillance zones |
| **Contact Tracing** | Identification of linked farms | 30-day traceback window; farms under enhanced surveillance |

### Control Zones

Three nested zones enforce differential surveillance and intervention intensity:

- **Infected zone** (3 km): Focus of depopulation and immediate control
- **Buffer zone** (7 km): Secondary vaccination prioritization
- **Surveillance zone** (15 km): Active detection and monitoring

---

## Key Features

✓ **Multi-host dynamics**: Explicit modeling of cattle, swine, and small ruminants  
✓ **Species-specific transmission**: Parameterized from empirical literature  
✓ **Stochastic processes**: Incorporates uncertainty in transmission, detection, and control efficacy  
✓ **Spatial heterogeneity**: Distance-dependent transmission and realistic farm networks  
✓ **Flexible control actions**: Customizable intervention timing, capacity, and extent  
✓ **Diagnostic imperfection**: Reflects real-world detection limitations  
✓ **Vital dynamics**: Optional incorporation of births and deaths  
✓ **Scenario analysis**: Supports evaluation of multiple strategies  

---

## Applications

MHASpread has been applied to:

- **FMD preparedness in South America**: Evaluating control strategies for Brazil (2024) and Bolivia (2023)
- **Cost-effectiveness analysis**: Integrating epidemiological and economic models to inform policy
- **Surveillance optimization**: Assessing detection capacity and traceback effectiveness
- **International training**: Building epidemiological modeling expertise in partner countries

---

## Events and Training

The MHASpread framework has been instrumental in international workshops and capacity-building initiatives aimed at strengthening disease surveillance and control expertise among veterinary and public health authorities in Latin America.

### Chile FMD Workshop 2024
[workshop_aftosa_chile_2024](https://machado-lab.github.io/workshop_aftosa_chile_2024/)  
*Training program for Chilean national authorities on FMD simulation modeling, risk-based surveillance design, and emergency response preparedness. Participants gained hands-on experience with MHASpread to evaluate control strategies tailored to Chilean agricultural contexts.*

### PANAFTOSA Workshop Rio 2023
[PANAFTOSA-Workshop-Rio2023](https://machado-lab.github.io/PANAFTOSA-Workshop-Rio2023/)  
*Regional capacity-building initiative hosted by PAHO's Pan American Animal Health Organization, bringing together epidemiologists and veterinary officials from across the Americas. Focus on metapopulation modeling, uncertainty quantification, and evidence-based contingency planning for transboundary animal diseases.*

### PAHO MHASpread Workshop Repository
[MHASpread_workshop_PAHO](https://github.com/machado-lab/MHASpread_workshop_PAHO/)  
*Comprehensive open-access workshop materials including tutorials, datasets, and worked examples. Provides reproducible workflows for disease simulation, sensitivity analysis, and policy evaluation using MHASpread-based methodologies.*

---

## Relationship to Published Work

MHASpread development and application are documented in peer-reviewed publications:

- **Cespedes Cardenas & Machado (2024)**: [Modeling foot-and-mouth disease dissemination in Brazil and evaluating the effectiveness of control measures](https://doi.org/10.3389/fvets.2024.1468864) — *Frontiers in Veterinary Science*

- **Cardenas et al. (2024)**: [Integrating epidemiological and economic models to estimate the cost of simulated foot-and-mouth disease outbreaks in Brazil](https://doi.org/10.1016/j.prevetmed.2025.106558) — *Preventive Veterinary Medicine*

- **Cespedes & Castillo (2023)**: [Foot-and-mouth disease in Bolivia: Simulation-based assessment of control strategies and vaccination requirements](https://doi.org/10.1155/tbed/9055612) — *Transboundary and Emerging Diseases*

---

## Limitations

- **Spatial scope**: Model designed for farm-level networks; may require adaptation for larger-scale systems
- **Data requirements**: Accurate population sizes, movement records, and species composition needed
- **Parameter uncertainty**: FMD transmission parameters derived from limited field studies; sensitivity analysis recommended
- **Control realism**: Model assumes adherence to protocol; deviations may affect outcomes
- **Single-pathogen focus**: Not designed for multi-pathogen coinfection scenarios

---

## Citation

If you use MHASpread in your research, please cite:

> Cespedes Cardenas, N., & Machado, G. (2024). Modeling foot-and-mouth disease dissemination in Brazil and evaluating the effectiveness of control measures. *Frontiers in Veterinary Science*, 11, 1468864. https://doi.org/10.3389/fvets.2024.1468864

---

## License

This repository contains documentation and educational materials for MHASpread. The proprietary model source code is provided under a separate commercial or institutional license agreement. See [LICENSE.md](MHASPREAD-model/LICENSE.md) for details.

---

## Acknowledgments

MHASpread development is supported by:

- **FUNDESA RS** (Fundação de Desenvolvimento para Excelência em Ciência e Tecnologia do Rio Grande do Sul)
- **NC State University** - Department of Population Health and Pathobiology
- **LUMAC** (Laboratório de Unidades Multidisciplinares de Apoio Científico) — Universidade Federal de Santa Maria

---

## Developers

🖥️ **Nicolas Cespedes Cardenas** [![ORCID](https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png)](https://orcid.org/0000-0001-7884-2353)  
*Machado Lab, College of Veterinary Medicine*  
*NC State University*

🖥️ **LUMAC Team**  
*Universidade Federal de Santa Maria, Brazil*

---

## Documentation

For detailed technical documentation, visit our [comprehensive documentation site](https://machado-lab.github.io/MHASPREAD-docs/) or explore the sections below:

- **[Model Overview](docs/model_overview.md)** — Detailed model structure and compartments
- **[Transmission Dynamics](docs/transmission_dynamics.md)** — Within-farm and spatial transmission mechanisms
- **[Control Strategies](docs/control_strategies.md)** — Detailed description of interventions
- **[Economic Impact](docs/economic_impact.md)** — Cost-effectiveness integration
- **[Data Requirements](docs/data_requirements.md)** — Input data specifications and preparation
- **[Events & Training](docs/events.md)** — International workshops and capacity-building
- **[Vignettes](vignettes/)** — Conceptual tutorials and use cases

