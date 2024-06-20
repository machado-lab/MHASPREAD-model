# MHASpread:  A multi-host Animal Spread Stochastic Multilevel Model(version 2.5.0) 

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

```R
# Plot infected farms curves considering all host species
plot_infected_farms_curve(model_output = resultado, host = "All host")
ggsave(last_plot(), file = "plot_infected_farms_curve_all.png", width = 10, height = 8)

# Plot infected farms curves considering bovine species
plot_infected_farms_curve(model_output = resultado, host = "Bovine")
ggsave(last_plot(), file = "plot_infected_farms_curve_bov.png", width = 10, height = 8)

# Plot infected farms curves considering swine species
plot_infected_farms_curve(model_output = resultado, host = "Swine")
ggsave(last_plot(), file = "plot_infected_farms_curve_swi.png", width = 10, height = 8)

# Plot infected farms curves considering small ruminants species
plot_infected_farms_curve(model_output = resultado, host = "Small ruminants")
ggsave(last_plot(), file = "plot_infected_farms_curve_small.png", width = 10, height = 8)

```

#### 4. Initial Spread Epidemic Curves of Animals

Similar to the previous section, this section generates and saves plots of infected animals over time, considering different host species.

```R
# Plot infected animal curves considering all host species
plot_SEIR_animals(model_output = resultado, plot_suceptible_compartment = FALSE, by_host = FALSE)
ggsave(last_plot(), file = "plot_SEIR_animals_by_host.png", width = 10, height = 8)

# Plot infected animal curves considering by host species
plot_SEIR_animals(model_output = resultado, plot_suceptible_compartment = FALSE, by_host = TRUE)
ggsave(last_plot(), file = "plot_SEIR_animals_all.png", width = 10, height = 8)

```

#### 5. Explore the hot spot areas of the disease

```
# Explore the geo-location of the farms
farms_location <- plot_nodes_kernel_map(model_output = resultado, population = population, initial_infected_farm = 666)
farms_location
# Take a snapshot of the map
mapview::mapshot(farms_location, file = "initial_outbreak_farms_location.png")  # Save the map
```


#### 6. Control Zones Areas

This part of the code defines control zones and areas based on the simulation results. It calculates and plots control zones around infected farms.

```R
detected_farms.id <- MHASpread::id_of_infectious_farms(
  resultado$populationdb[resultado$populationdb$day == max(resultado$populationdb$day), ],
  only_infected_comp = TRUE
)

zones_arond_inft_farms <- assign_control_zones(
  population = population,
  infected_size = 3/2,
  buffer_size = 7/2,
  surveillance_size = 15/2,
  detected_farms.id = detected_farms.id,
  num_threads = 1
)

# Plot interactive map for control zones
plot_farms_in_control_zones_areas(zones_arond_inft_farms, detected_farms.id)
```

#### 7. Control Action Modeling

This section runs control actions based on the simulation results. It models various control measures, such as depopulation, vaccination, and movement restrictions. 

```R
control_model <- control_actions_sim(
  # MODEL SETUP
  model_output = resultado,               # Output object of the function stochastic_SEIR()
  population_data = population,   # Naive population  before the simulations
  events = events,                # Initial Scheduled movements
  break_sim_if = 50,                         # Breaks the simulation if there ar more than n infectious farms
  visits = 50,                                # number of visits that SVO can do in one day
  first_detectn_proportion = 0.1,             # proportion that represents

  # INITIAL CONDITION OF THE CONTROL ACTIONS
  days_of_control_action = 90,               # Number of days to be working on control actions

  # CONTROL ZONES AREAS SETUP
  infected_size_cz = 3,                      # Ratio size in Km of the infected zone
  buffer_size_cz = 7,                        # Ratio size in Km of the buffer zone
  surveillance_size_cz = 15,                 # Ratio size in Km of the surveillance zone

  # ANIMAL MOVEMENTS STANDSTILL SETUP
  ban_length = 30,                           #  30 days of movements ban
  infected_zone_mov = T,                     #  Animal ban will be applied to infected zone
  buffer_zone_mov = T,                       #  Animal ban will be applied to buffer zone
  surveillance_zone_mov = T,                 #  Animal ban will be applied to surveillance zone
  direct_contacts_mov = T,                   #  Ban farm outside of control zones with contact with positive farms
  traceback_length_mov = 1,                  #  Traceback in-going animals movements of infected farms

  # DEPOPULATION SETUP
  limit_per_day_farms_dep = 5,               #  Farm will be depopulated by day
  infected_zone_dep = T,                     #  Depopulation will be applied to infected zone
  only_depop_infect_farms = T,               #  If False stamping out all farms in the infected zone

  # VACCINATION SETUP
  days_to_get_inmunity = 7,                 # How many days to be considered 100% immune
  limit_per_day_farms = 50,                  # Maximum number of farms to be vaccinated in BUFFER area
  limit_per_day_farms_infct = 50,            # Maximum number of farms to be vaccinated in INFECTED area
  vacc_eff = 0.9,                            # Numeric value between 0 and 1 indicating the efficacy of the vaccine
  dt = 1/7,                                 # Rate of conversion to SEIR -> V compartment i.e 1/15
  vacc_swine = F,                            # If true vaccine swine
  vacc_bovine = T,                           # If true vaccine bovine
  vacc_small = F,                            # If true vaccine small ruminants
  infected_zone_vac =T,                     # If true vaccine over infected control zone area
  buffer_zone_vac = T,                       # If true vaccine over buffer control zone area
  vacc_infectious_farms =   T,               # If true infectious farms will be vaccinated
  vacc_delay = 0)                            # How many days until start the vaccination

```

 Plot epidemic curves and control action results for all farm types

 ```R
plot_epi_curve_mean_and_cntrl_act(
  model_inital = resultado,
  model_control = control_model,
  control_action_start_day = min(control_model[[2]]$population_by_day$day),
  plot_only_total_farms = TRUE
)

#save the plot
ggsave(last_plot(), file = "plot_epi_curve_mean_and_cntrl_act_all_farm.png", width = 10, height = 8)
```

#### 8. Depopulated Farms Distribution

This section generates plots showing the distribution of depopulated farms and animals throughout the simulation.

```R
# Let's see the results of the depopulated farms' overall simulation
plot_depopulation(control_output = control_model, level_plot = "farms")
ggsave(last_plot(), file = "plot_depopulation_farm.png", width = 10, height = 8)

# Let's see the results of the depopulated animals' overall simulation
plot_depopulation(control_output = control_model, level_plot = "animals")
ggsave(last_plot(), file = "plot_depopulation_animal.png", width = 10, height = 8)


### lest explore the cost of depopulation based on the previous results
plot_depopulation_cost(control_output = control_model,
                       level_plot = "animals", cost = 250,
                       cumulative = T)
ggsave(last_plot(), file = "plot_depopulation_cost_ani.png", width = 10, height = 8)

plot_depopulation_cost(control_output = control_model,
                       level_plot = "farms", cost = 1000,
                       cumulative = T)
ggsave(last_plot(), file = "plot_depopulation_cost_farm.png", width = 10, height = 8)

```

#### 9. Vaccinated Results of Control Actions

In this part of the code, the results of vaccination actions are plotted, showing the impact on farms and animals over time.

```R
plot_vaccination(control_output = control_model, population = population, level_plot = "farms")
ggsave(last_plot(), file = "plot_vaccination_farms.png", width = 10, height = 8)

# Let's see the results of the vaccinated animals' overall simulation

plot_vaccination(control_output = control_model, population = population, level_plot = "animals")
ggsave(last_plot(), file = "plot_vaccination_animals.png",  width = 10, height = 8)
### lest explore the cost of vaccination based of the previous results
plot_vaccination_cost(control_output = control_model, population = population,
                      level_plot = "animals", cost = 5, cumulative = F)

ggsave(last_plot(), file = "plot_vaccination_cost_animals.png",  width = 10, height = 8)

plot_vaccination_cost(control_output = control_model, population = population,
                      level_plot = "farms", cost = 5, cumulative = T)
ggsave(last_plot(), file = "plot_vaccination_cost_farms.png",  width = 10, height = 8)
```

#### 10. Calculate the Number of Staff Members

This section calculates the number of staff members required for depopulation and vaccination actions and plots the results.

```R

plot_staff_overhead(control_output = control_model,
                    population = population, parameter = "depopulation",
                    staff  = 2)

# Number of staff for vaccination

plot_staff_overhead(control_output = control_model,
                    population = population, parameter = "vaccination",
                    staff  = 1)
```

### Running the Code

To run the code, execute each section sequentially in an R environment. Make sure to adjust the parameters and file paths as needed for your specific scenario.

## License

This code is provided under the [Proprietari License](LICENSE.md).

## Acknowledgments

The disease spread simulation and control actions code are part of the MHASpread package developed by the Machado Lab. Special thanks to the contributors and developers of the package.

Feel free to explore and adapt this code for your disease spread modeling and control research. If you have any questions or issues, please open an [issue](https://github.com/your/repository/issues) on this GitHub repository.



## Authors
Nicolas Cardenas [![ORCIDiD](https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png)](https://orcid.org/0000-0001-7884-2353) <br />
Gustavo Machado [![ORCIDiD](https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png)](https://orcid.org/0000-0001-7552-6144)

## Developers
:computer: Nicolas Cardenas [![ORCIDiD](https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png)](https://orcid.org/0000-0001-7884-2353) at the [Machado lab](https://machado-lab.github.io/) 
:computer: [Universidade Federal de Santa Maria](https://www.ufsm.br/orgaos-de-apoio/sai/welcome-to-ufsm)

## :muscle: Sponsors
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/fundesalogo.jpg?raw=true" align="left" width="200" ></a>
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/ncstate-type-4x1-red-min.png?raw=true" align="left" width="200" ></a>
<a href="url"><img src="https://github.com/ncespedesc/logos_nc_state/blob/main/seapilogo.png?raw=true" align="left" width="300" ></a>
