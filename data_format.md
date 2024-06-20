# MHASpread: A Multi-host Animal Spread Stochastic Multilevel Model (Version 3.0.0)

![generic](https://img.shields.io/badge/Control_actions-up-green) ![generic1](https://img.shields.io/badge/Spatial_transmission-up-green) ![generic2](https://img.shields.io/badge/Network_level-up-green) ![generic3](https://img.shields.io/badge/Vital_dynamics-Up-green) ![generic4](https://img.shields.io/badge/SEIR_model-Up-green)

## Getting Started

The MHASpread package works with two main files:

1. A file containing population data, `population`.
2. A file containing movement data, `events`, which must be created before running the model.

### Population Database

The `population` database is a data frame object where each line represents a node (farm, premises, municipality), and the columns denote the features of this node. The table below describes the meaning of each column in the population database.

**Note:** To prepare the data, the number of animals should be added to the susceptible compartment, which means it is required to complete the columns **S\_bov\_pop**, **S\_swi\_pop**, and **S\_small\_pop** with the number of animals on each farm according to the host species. Farms without animals of a specific species should be completed with 0.

Please ensure that the geolocation of the farms is provided in degrees in the columns **longitude** and **latitude**, as well as in UTM in the columns **long** and **lat**.

| **Column**        | **Description**                                                  |
|-------------------|-------------------------------------------------------------------|
| **node**          | Represents the unit of observation (e.g., farm or slaughterhouse) |
| **latitude**      | Latitude of the node in degrees                                   |
| **longitude**     | Longitude of the node in degrees                                  |
| **S\_bov\_pop**   | Susceptible bovine population                                     |
| **E\_bov\_pop**   | Exposed bovine population                                         |
| **I\_bov\_pop**   | Infected bovine population                                        |
| **R\_bov\_pop**   | Recovered bovine population                                       |
| **V\_bov\_pop**   | Vaccinated bovine population                                      |
| **D\_bov\_pop**   | Depopulated bovine population                                    |
| **S\_swi\_pop**   | Susceptible swine population                                      |
| **E\_swi\_pop**   | Exposed swine population                                          |
| **I\_swi\_pop**   | Infected swine population                                         |
| **R\_swi\_pop**   | Recovered swine population                                        |
| **V\_swi\_pop**   | Vaccinated swine population                                       |
| **D\_swi\_pop**   | Depopulated swine population                                     |
| **S\_small\_pop** | Susceptible small ruminants population                            |
| **E\_small\_pop** | Exposed small ruminants population                                |
| **I\_small\_pop** | Infected small ruminants population                               |
| **R\_small\_pop** | Recovered small ruminants population                              |
| **V\_small\_pop** | Vaccinated small ruminants population                              |
| **D\_small\_pop** | Depopulated small ruminants population                            |
| **long**          | Longitude of the node in Universal Transverse Mercator (UTM)       |
| **lat**           | Latitude of the node in Universal Transverse Mercator (UTM)        |

### Events Database

The `events` database is a data frame object where each line represents a movement of animals between nodes. This dataframe includes the specific number of animals moved, the species, and the timestep related to each movement.

| **Column**      | **Description**                                                                                                                                                 |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **date**        | Timestep when the event occurs                                                                                                                                   |
| **event\_type** | Type of event:<br/> **1**: Farm-to-farm movements<br/> **2**: Movements to slaughterhouses<br/> **3**: Births of animals<br/> **4**: Deaths of animals   |
| **from**        | ID of the origin farm/premise                                                                                                                                    |
| **to**          | ID of the destination farm/premise                                                                                                                               |
| **host**        | Host species being moved (options: Bovine, Swine, Small ruminants)                                                                                              |
| **number**      | Number of animals related to the event                                                                                                                           |

---
