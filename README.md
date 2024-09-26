# CABLE Offline Configuration

## ABOUT
This repository contains the CABLE offline configurations used as part of the [Global Carbon Budget](https://www.globalcarbonproject.org/carbonbudget/) project. These configurations are the most complicated configuration currently being run with CABLE, containing a series of spin-up stages. The set of stages used in a given configuration is set by the ```stage_config.yaml``` file. The stages in this configuration are as follows:

1. Climate spin-up (```climate_spinup```)
2. Biomass spin-up (```biomass_spinup```)
3. Carbon stocks spin-up with unrestricted labile
    A. Numerical (```unrestricted_N_P```)
    B. Analytical (```unrestricted_N_P_analytic```)
4. Carbon stocks spin-up with restricted labile
    A. Numerical (```restricted_N_P```)
    B. Analytical (```restricted_N_P_analytic```)
5. Land use change initialisation (only with dynamic land use) (```initialise_land_use```)
6. Spin-up with all physics modules active (```full_dynamic_spinup```)
7. Final run (```final_run```)

The sub-stages within stages 3 and 4 are cycled based on their entries in ```stage_config.yaml```. The configurations are named based on the scenarios specified by the Global Carbon Budget's [TRENDY](https://blogs.exeter.ac.uk/trendy/) project. 

## Supported Configurations

The supported configuration reflect the respective TRENDY scenarios. The scenarios and respective branches are:

* [Scenario 0 (S0)](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S0): Pre-industrial atmospheric CO2 and nitrogen deposition, with recycled meteorology from 1901 to 1920 and static land use.
* [Scenario 1 (S1)](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S1): Yearly atmospheric CO2 and nitrogen deposition, with recycled meteorology from 1901 to 1920 and static land use.
* [Scenario 2 (S2)](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S0): Yearly atmospheric CO2 and nitrogen deposition, with yearly meteorology and static land use.
* [Scenario 3 (S3)](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S0): Yearly atmospheric CO2 and nitrogen deposition, with yearly meteorology and dynamic land use.

Note that in this instance the differences between the scenarios are restricted to the ```full_dynamic_spinup``` and ```centennial run``` stages. The prior spin-up stages are the same across scenarios (bar ```intialise_land_use```, which is only active in scenario 3). The meteorology used is the [CRU-JRA v2.4](https://catalogue.ceda.ac.uk/uuid/aed8e269513f446fb1b5d2512bb387ad/) dataset. The land use change data used is the [LUH2](https://luh.umd.edu/) dataset. Both are downscaled to 1 degree resolution.

## Scenario Details

### Atmospheric CO2 and Nitrogen Deposition

In this instance, "pre-industrial" refers to the year 1700 for the atmospheric CO2 and 1850 for the nitrogen deposition. For scenarios 1 through 3, the yearly data is used for the ```full_dynamic_spinup``` and ```final_run``` stages. The source of these datasets is currently unknown.

### Meteorology

The time period used for the recycled meteorology is a 20 year period from 1901 to 1920. The recycled meteorology is used for all stages bar the ```final_run``` stages in scenarios 2 and 3.

### Land Use Change

The time period used for the ```initialise_land_use``` stage in scenario 3 is 1580 to 1699, and 1700 to 1900 for the ```full_dynamic_spinup``` stage.
