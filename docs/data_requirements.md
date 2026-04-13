[← Return to Documentation](index.md)

---

# Data Requirements

This page specifies the data inputs, formats, and preparation procedures required to run MHASpread simulations.

---

## Overview

MHASpread requires two primary input datasets:

1. **Population Data**: Farm-level characteristics (location, animal counts, species composition)
2. **Events Data**: Animal movements between farms over the simulation period

Additionally, users specify **scenario parameters** (control action capacities, detection parameters, vaccine efficacy).

---

## Population Database

### Structure

The population database is a data frame where each row represents a single farm (node), and columns encode farm characteristics.

### Required Columns

#### Geographic Location (Required)

| Column | Type | Units | Description | Example |
|---|---|---|---|---|
| `node` | character/integer | ID | Unique farm identifier | "Farm_001", 1 |
| `latitude` | numeric | degrees | Geographic latitude (WGS84) | -29.456 |
| `longitude` | numeric | degrees | Geographic longitude (WGS84) | -56.123 |
| `lat` | numeric | meters | UTM latitude (Zone-specific) | 6725000 |
| `long` | numeric | meters | UTM longitude (Zone-specific) | 450000 |

**Note**: Provide coordinates in both geographic (degrees) and projected (UTM) formats. The model uses kilometers for distance calculations; UTM provides better metric accuracy than degrees at regional scales.

#### Disease Status & Compartments (Cattle)

| Column | Type | Animal Count | Description |
|---|---|---|---|
| `S_bov_pop` | integer | Count | Susceptible cattle |
| `E_bov_pop` | integer | Count | Exposed cattle |
| `I_bov_pop` | integer | Count | Infectious cattle |
| `R_bov_pop` | integer | Count | Recovered cattle |
| `V_bov_pop` | integer | Count | Vaccinated cattle |
| `D_bov_pop` | integer | Count | Depopulated cattle (usually 0 at start) |

#### Disease Status & Compartments (Swine)

| Column | Type | Animal Count | Description |
|---|---|---|---|
| `S_swi_pop` | integer | Count | Susceptible swine |
| `E_swi_pop` | integer | Count | Exposed swine |
| `I_swi_pop` | integer | Count | Infected swine |
| `R_swi_pop` | integer | Count | Recovered swine |
| `V_swi_pop` | integer | Count | Vaccinated swine |
| `D_swi_pop` | integer | Count | Depopulated swine |

#### Disease Status & Compartments (Small Ruminants)

| Column | Type | Animal Count | Description |
|---|---|---|---|
| `S_small_pop` | integer | Count | Susceptible small ruminants |
| `E_small_pop` | integer | Count | Exposed small ruminants |
| `I_small_pop` | integer | Count | Infectious small ruminants |
| `R_small_pop` | integer | Count | Recovered small ruminants |
| `V_small_pop` | integer | Count | Vaccinated small ruminants |
| `D_small_pop` | integer | Count | Depopulated small ruminants |

### Data Preparation Guidelines

#### Initialization Convention

At **simulation start (day 0)**:

- All animals are placed in susceptible compartments (`S_*_pop`)
- All other compartments (`E_*, I_*, R_*, V_*, D_*`) are typically **zero**
- **Exception**: If simulating disease already present, populate `I_*_pop` for index farms

#### Species Composition Rules

1. **Farms without a species**: Enter `0` for all compartments of that species
2. **Zero-animal farms**: Include in database but exclude from epidemiologically active subset
3. **Farm-level totals**: No constraint (species counts independent)

#### Coordinate Accuracy

- **Geographic (lat/long)**: Required; use GPS or cadastral records with ≥1 decimal place (±1 km precision minimum)
- **UTM coordinates**: Derived from geographic; ensure **correct UTM zone** for study region
  - Example: Southern Brazil (Rio Grande do Sul) typically uses UTM Zone 22S or 21S
  - Verify zone in projection metadata

#### Population Data Sources

- **Census data**: National cattle/swine registries
- **Surveys**: Veterinary authorities, farmer registrations
- **Remote sensing**: Spatial livestock density estimates
- **Modeling**: Where direct data unavailable, use census-modeled estimates

---

## Events Database

### Structure

The events database is a data frame where each row represents a single animal movement event between two farms.

### Required Columns

| Column | Type | Units | Description | Example |
|---|---|---|---|---|
| `date` | date | YYYY-MM-DD | Date of shipment | "2024-01-15" |
| `sender` | character/integer | ID | Origin farm (must match `population$node`) | "Farm_001" |
| `receiver` | character/integer | ID | Destination farm (must match `population$node`) | "Farm_042" |
| `species` | character | Code | Species moved: "bov" (cattle), "swi" (swine), "small" (small ruminants) | "swi" |
| `count` | integer | Number | Quantity of animals moved | 50 |

### Data Preparation Guidelines

#### Date Range

- **Start date**: First movement or simulation start (days before outbreak)
- **End date**: Latest movement or end of simulation period
- **Resolution**: Daily (one row per shipment)

#### Sender-Receiver Validation

Both `sender` and `receiver` must exactly match values in `population$node`:

- **Case-sensitive**: "Farm_001" ≠ "farm_001"
- **Typos**: Will cause errors—validate against population database
- **Bidirectional movements**: A farm can appear as both sender and receiver

#### Species Codes

Use standardized codes:

- `"bov"`: Cattle
- `"swi"`: Swine
- `"small"`: Small ruminants

**Validation**: Compare species records against farm population data (don't ship swine from a cattle-only farm).

#### Movement Types

MHASpread treats all movements identically:

- Sale/trade movements
- Restocking
- Breeding stock transfers
- Lease animals (typically excluded in real surveillance, but can be included)

#### Missing Movement Data

If movements not comprehensively recorded:

1. **Partial data**: Use available records (may underestimate network transmission)
2. **Model validation**: Compare simulation to observed with subsampled movements
3. **Sensitivity analysis**: Test scenarios with/without network transmission

---

## Scenario Parameters

Beyond population and events data, specify control scenario parameters in configuration files or R script:

### Detection Parameters

| Parameter | Type | Range | Default | Meaning |
|---|---|---|---|---|
| `detection_lag` | numeric | 0–10 days | ~3 days | Delay before detection after surveillance triggered |
| `diagnostic_sensitivity` | numeric | 0–1 | 1.0 | Probability farm with infected animals is detected |
| `surveillance_active` | logical | TRUE/FALSE | TRUE | Activate active surveillance in zones |

### Depopulation Parameters

| Parameter | Type | Range | Default | Meaning |
|---|---|---|---|---|
| `depop_capacity_day` | integer | 1–100 | 3 | Farms depopulated per day |
| `depop_priority` | character | "size", "date" | "size" | Ordering criterion (largest first, or by detection date) |

### Vaccination Parameters

| Parameter | Type | Range | Default | Meaning |
|---|---|---|---|---|
| `vax_capacity_day` | integer | 1–50 | 5 | Farms vaccinated per day |
| `vax_lag_days` | integer | 0–20 | 15 | Days from zone establishment to vaccination start |
| `vax_efficacy` | numeric | 0–1 | 0.95 | Probability vaccinated animal becomes immune |
| `vax_duration_farm` | integer | 5–30 | 15 | Days to vaccinate entire farm |

### Spatial/Temporal Parameters

| Parameter | Type | Units | Default | Meaning |
|---|---|---|---|---|
| `silent_spread_days` | integer | days | 20 | Pre-detection spread period |
| `infected_zone_radius` | numeric | km | 3 | Depopulation zone radius |
| `buffer_zone_radius` | numeric | km | 7 | Vaccination zone radius |
| `surveillance_zone_radius` | numeric | km | 15 | Active detection zone radius |
| `standstill_duration` | integer | days | 30 | Movement restriction period |

### Outbreak Initialization

| Parameter | Type | Description |
|---|---|---|
| `index_farms` | vector of IDs | Farm(s) where infection starts (must be in population$node) |
| `index_infected_animals` | integer per species | Initial infectious animals at index farm(s) |

---

## Quality Assurance Checklist

Before running simulations, verify:

### Population Data

- [ ] All farms have unique `node` identifiers
- [ ] Coordinates present and in correct format (decimal degrees)
- [ ] UTM zone correctly specified
- [ ] No missing values (NA) in required columns
- [ ] Animal counts non-negative integers
- [ ] At least one species per farming region exists
- [ ] Total animal populations plausible for region

### Events Data

- [ ] All `sender` and `receiver` IDs match population database
- [ ] Dates in ISO format (YYYY-MM-DD), chronologically ordered
- [ ] Species codes are "bov", "swi", or "small"
- [ ] Movement counts positive integers
- [ ] No duplicate events (same date, sender, receiver, species)
- [ ] No movements to/from non-existent farms
- [ ] Species movements plausible (e.g., don't ship cattle from cattle-only farm)

### Scenario Parameters

- [ ] Control capacities ≥ 1 farm/day
- [ ] Efficacy parameters between 0 and 1
- [ ] Lag times reasonable (≤ infectious period)
- [ ] Zone radii: infected < buffer < surveillance
- [ ] Index farms exist in population database
- [ ] Initial infected counts ≤ farm animal populations

---

## File Formats

### CSV (Recommended)

```
node,latitude,longitude,lat,long,S_bov_pop,E_bov_pop,I_bov_pop,...
Farm_001,-29.456,-56.123,6725000,450000,150,0,0,...
Farm_002,-29.401,-56.089,6730000,451000,200,0,0,...
```

### R Data Format (.rds or .RData)

Save as native R data frames for direct loading:

```r
population <- read.csv("population.csv")
events <- read.csv("events.csv")
saveRDS(list(population = population, events = events), "simulation_data.rds")
```

### Excel (.xlsx)

Multiple sheets:

- Sheet 1: `population` (with all columns)
- Sheet 2: `events` (with all columns)

---

## Documentation Template

Archive simulation inputs with metadata:

```
project_name/
├── input/
│   ├── population.csv
│   ├── events.csv
│   └── scenario_parameters.R
├── metadata/
│   ├── README.md (data source, quality notes)
│   ├── data_dictionary.txt (column definitions)
│   └── coordinate_reference.txt (UTM zone, datum)
└── README_inputs.md (data preparation log)
```

---

## Common Errors & Solutions

| Error | Cause | Solution |
|---|---|---|
| "Sender_ID not found" | Typo in events $sender | Check against population $node exactly |
| "Negative animals" | Data entry error | Validate animal counts ≥ 0 |
| "Land outside simulation region" | Coordinate error | Verify lat/long in study region bounds |
| "Duplicate rows" | Data duplication | Check for repeated entries |
| "Implausibly large movement" | Data error | Validate against farm population size |

---

## Next Steps

- For guidance on running simulations with prepared data, see **[Model Structure Vignette](../vignettes/model_structure.md)**
- For example data format, see the [example_script.md](../example_script.md)
- For uncertainty quantification, see **[Model Overview](model_overview.md)**

