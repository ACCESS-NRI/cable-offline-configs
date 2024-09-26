# CABLE Offline Configuration

## ABOUT
This repository contains the CABLE offline configurations used as part of the [Global Carbon Budget](https://www.globalcarbonproject.org/carbonbudget/) project. These configurations are the most complicated configuration currently being run with CABLE, containing a series of spin-up stages. The set of stages used in a given configuration is set by the ```stage_config.yaml``` file. The stages in this configuration are as follows:

1. Climate spin-up
2. Biomass spin-up
3. Carbon stocks spin-up with unrestricted labile
    A. Numerical
    B. Analytical
4. Carbon stocks spin-up with restricted labile
    A. Numerical
    B. Analytical
5. Land use change initialisation (only with dynamic land use)
6. Spin-up with all physics modules active
7. Final run

The sub-stages within stages 3 and 4 are cycled based on their entries in ```stage_config.yaml```. The configurations are named based on the scenarios specified by the Global Carbon Budget's [TRENDY](https://blogs.exeter.ac.uk/trendy/) project. 

## Supported Configurations

The supported configuration reflect the respective TRENDY scenarios. The scenarios and respective branches are:

* [Scenario 0 (S0)](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S0): Pre-industrial atmospheric CO2 and Nitrogen deposition, with recycled climatology from 1901 to 1920 and static land use.
* [Scenario 1 (S1)](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S1): Yearly atmospheric CO2 and Nitrogen deposition, with recycled climatology from 1901 to 1920 and static land use.
* [Scenario 2 (S2)](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S0): Yearly atmospheric CO2 and Nitrogen deposition, with yearly climatology and static land use.
* [Scenario 3 (S3)](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S0): Yearly atmospheric CO2 and Nitrogen deposition, with yearly climatology and dynamic land use.

The climatology used is the [CRU-JRA v2.4](https://catalogue.ceda.ac.uk/uuid/aed8e269513f446fb1b5d2512bb387ad/) dataset. The land use change data used is the [LUH2](https://luh.umd.edu/) dataset. Both are downscaled to 1 degree resolution.
