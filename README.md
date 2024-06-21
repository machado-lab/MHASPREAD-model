# MHASpread:  A multi-host Animal Spread Stochastic Multilevel Model(version 3.0.0) 

 ![generic4](https://img.shields.io/badge/SEIR_model-Up-green) ![generic3](https://img.shields.io/badge/Births_and_Deaths-Up-green) ![generic1](https://img.shields.io/badge/Spatial_Transmission-up-green) ![generic2](https://img.shields.io/badge/Animal_Movements-up-green) ![generic](https://img.shields.io/badge/Disease_Control_Actions-up-green)   

<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/MHASpread_logo.png?raw=true" align="left" height="100" width="100"></a>

# Model description
This is a multi-host, single-pathogen, coupled multiscale model designed to simulate epidemic trajectories of disease. It incorporates various customizable control actions specific to different zones (such as infected, buffer, and surveillance zones) to include depopulation, vaccination, and animal movement standstills, among other countermeasures. The model relies on two main sources of data: `population` and `events`. For a detailed description of the data format, please refer to [click here](data_format.md).

## Model outputs 
### Susceptible-Exposed(Latent)-Infectious-Recovered dynamics within a farm
Example of stochastic dynamics of a farm  with 100 animals 
<br />
<img width="500" alt="Screenshot 2024-06-20 at 1 45 52 PM" src="https://github.com/machado-lab/MHASPREAD-model/assets/41584216/bd556c86-8e72-43ca-94dd-1a437b9ef6d2">
<br />
#### Epidemic Curves of a Population Farms
Infected farms over time, considering different host species. 
<br />
<img width="500"  alt="Screenshot 2024-06-20 at 1 52 08 PM" src="https://github.com/machado-lab/MHASPREAD-model/assets/41584216/ebceeec0-47f9-40f4-b883-f39249ae40ad">
<br />
#### Number of animals in each compartment considering a population farms 
The number of infected animals for all host species(susceptible compartment was excluded from the plot to improve data visualization)
<br/>
<img width="500" alt="Screenshot 2024-06-20 at 1 57 20 PM" src="https://github.com/machado-lab/MHASPREAD-model/assets/41584216/e3abf095-b658-40ac-a6a8-d472443eb914">
<br/>
#### Control actions 
Evaluate the performance of the simulated control action 
<img width="500" alt="Screenshot 2024-06-21 at 1 17 43 PM" src="https://github.com/machado-lab/MHASPREAD-model/assets/41584216/d6604953-5da3-4b64-be86-220d00645d0d">


## How to use 
A complete example code is provided [here.](example_script.rm) 


### Running the Code

To run the code, execute each section sequentially in an R environment. Make sure to adjust the parameters and file paths as needed for your specific scenario.

## License

This model source is provided under the [Proprietari License](LICENSE.md).

## Acknowledgments

The disease spread simulation and control actions code are part of the MHASpread package developed by the Machado Lab. Special thanks to the contributors and developers of the package.

Feel free to explore and adapt this code for your disease spread modeling and control research. If you have any questions or issues, please open an [issue](https://github.com/your/repository/issues) on this GitHub repository.



## Authors
Nicolas Cardenas [![ORCIDiD](https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png)](https://orcid.org/0000-0001-7884-2353) <br />
Gustavo Machado [![ORCIDiD](https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png)](https://orcid.org/0000-0001-7552-6144)

## Developers
:computer: Nicolas Cardenas [![ORCIDiD](https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png)](https://orcid.org/0000-0001-7884-2353) at the [Machado lab](https://machado-lab.github.io/) and 
:computer: [Universidade Federal de Santa Maria](https://www.ufsm.br/orgaos-de-apoio/sai/welcome-to-ufsm)

## :muscle: Sponsors
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/fundesalogo.jpg?raw=true" align="left" width="200" ></a>
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/ncstate-type-4x1-red-min.png?raw=true" align="left" width="200" ></a>
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/seapilogo.png?raw=true" align="left" width="300" ></a>
