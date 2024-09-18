[//]: # (Author: Lachlan Whyborn)
[//]: # (Date Modified: )

This repository contains the CABLE offline configurations used as part of the [Global Carbon Budget](https://www.globalcarbonproject.org/carbonbudget/) project. There are 4 scenarios incorporated into this work:

* Scenario 0 (S0): Pre-industrial atmospheric CO2 and Nitrogen deposition, with recycled climatology from 1901 to 1920 and static land use.
* Scenario 1 (S1): Yearly atmospheric CO2 and Nitrogen deposition, with recycled climatology from 1901 to 1920 and static land use.
* Scenario 2 (S2): Yearly atmospheric CO2 and Nitrogen deposition, with yearly climatology and static land use.
* Scenario 3 (S3): Yearly atmospheric CO2 and Nitrogen deposition, with yearly climatology and dynamic land use.

The climatology used is the (CRU-JRA v2.4)[https://catalogue.ceda.ac.uk/uuid/aed8e269513f446fb1b5d2512bb387ad/] dataset. The land use change data used is the (LUH2)[https://luh.umd.edu/] dataset. Both are downscaled to 1 degree resolution.

This configuration employs a complex spin-up process, described below.
