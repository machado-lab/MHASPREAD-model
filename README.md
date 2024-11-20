# MHASpread: A Multi-Host Animal Spread Stochastic Multilevel Model (version 3.0.0)

![SEIR_model](https://img.shields.io/badge/SEIR_model-Up-green) ![Births_and_Deaths](https://img.shields.io/badge/Births_and_Deaths-Up-green) ![Spatial_Transmission](https://img.shields.io/badge/Spatial_Transmission-Up-green) ![Animal_Movements](https://img.shields.io/badge/Animal_Movements-Up-green) ![Disease_Control_Actions](https://img.shields.io/badge/Disease_Control_Actions-Up-green)   


# Model Description
MHASpread is a multi-host, single-pathogen, coupled multiscale model designed to simulate epidemic trajectories of diseases. It incorporates various customizable control actions specific to different zones (such as infected, buffer, and surveillance zones), including depopulation, vaccination, and animal movement standstills, among other countermeasures. The model relies on two main sources of data: `population` and `events`. For a detailed description of the data format, please refer to [data_format.md](data_format.md). An example of R Script about how to run [is presented here.](example_script.md)

## Model Outputs
### Susceptible-Exposed (Latent)-Infectious-Recovered Dynamics Within a Farm
Example of stochastic dynamics of a farm with 100 animals:
<br/>
<img width="500" alt="Screenshot 2024-06-20 at 1 45 52 PM" src="https://github.com/machado-lab/MHASPREAD-model/assets/41584216/bd556c86-8e72-43ca-94dd-1a437b9ef6d2">
<br/>

### Epidemic Curves of Population Farms
Infected farms over time, considering different host species:
<br/>
<img width="500" alt="Screenshot 2024-06-20 at 1 52 08 PM" src="https://github.com/machado-lab/MHASPREAD-model/assets/41584216/ebceeec0-47f9-40f4-b883-f39249ae40ad">
<br/>

### Number of Animals in Each Compartment Considering Population Farms
Number of infected animals for all host species (the susceptible compartment is excluded to improve data visualization):
<br/>
<img width="500" alt="Screenshot 2024-06-20 at 1 57 20 PM" src="https://github.com/machado-lab/MHASPREAD-model/assets/41584216/e3abf095-b658-40ac-a6a8-d472443eb914">
<br/>

### Control Actions 
Evaluate the performance of the simulated control actions:
<br/>
<img width="500" alt="Screenshot 2024-06-21 at 1 17 43 PM" src="https://github.com/machado-lab/MHASPREAD-model/assets/41584216/d6604953-5da3-4b64-be86-220d00645d0d">
<br/>

## License
This model source is provided under the [Proprietary License](LICENSE.md).

## Citation 
The full model is [described here](https://www.frontiersin.org/journals/veterinary-science/articles/10.3389/fvets.2024.1468864/full).

## Acknowledgments
This model is funded by FUNDESA RS.

## Developers
:computer: Nicolas Cardenas [![ORCIDiD](https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png)](https://orcid.org/0000-0001-7884-2353) at the [Machado Lab](https://machado-lab.github.io/) and <br />
:computer: LUMAC team at [Universidade Federal de Santa Maria](https://www.ufsm.br/orgaos-de-apoio/sai/welcome-to-ufsm)

## :muscle: Sponsors
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/fundesalogo.jpg?raw=true" align="left" width="200"></a>
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/ncstate-type-4x1-red-min.png?raw=true" align="left" width="200"></a>
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/seapilogo.png?raw=true" align="left" width="300"></a>
