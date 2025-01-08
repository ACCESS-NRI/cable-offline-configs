# CABLE Offline Configurations

This repository contains important and/or common CABLE standalone configurations. The configurations are runnable with the [payu](https://github.com/payu-org/payu) tool. To use the CABLE driver in payu, set the model in ```config.yaml``` to ```model: staged_cable```. For information about the other options in the ```config.yaml``` file, see the ```payu``` documentation.

## Repository Structure

Each configuration is stored on a branch. The branches appended with ```_release``` are the configurations which we provide support for, and will be kept up-to-date with any advances in the CABLE code. The configurations supported at this time are:

* TRENDY: Configurations used to contribute to the [TRENDY](https://github.com/payu-org/payu) project. This project contains the full suite of CABLE science, including [CASA-CNP](https://www.access-nri.org.au/models/model-components/biogeochemistry-model-component-land/), tree demography and land use change. The TRENDY project considers a series of scenarios, each of which have their own branch in the repository. The scenarios are:
    - [S0](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S0): Pre-industrial CO2 and nitrogen deposition, recycled meteorology from 1901 to 1920 and static land use.
    - [S1](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S1): Yearly CO2 and nitrogen deposition, recycled meteorology from 1901 to 1920 and static land use.
    - [S2](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S2): Yearly CO2 and nitrogen deposition, yearly meteorology and static land use.
    - [S3](https://github.com/CABLE-LSM/cable-offline-configs/tree/release-trendy-S3): Yearly CO2 and nitrogen deposition, yearly meteorology and dynamic land use.
* Offline-as-ACCESS: Configuration intended to run CABLE as if it were in the ACCESS-ESM coupled model.

## How the Configurations Work

CABLE configurations typically consist of multiple stages, used to spin up progressively more aspects of the science into pseudo-equilibrium. To manage the staged execution of CABLE with changing namelist options, the following approach is used.

### Setting up the Configuration

The stages contained in a given configuration are defined in the ```stage_config.yaml``` file. A stage is an entry in the ```yaml``` file, with the ```count``` entry for each stage defining the number of times to run the given stage. A simple example of a ```stage_config.yaml``` with 3 stages each running 1 time would look like:

```stage_config.yaml```
```yaml
climate_spinup:
    count: 1
biomass_spinup:
    count: 1
final_run:
    count: 1
```

The stages run in the order they are defined, so this would run ```climate_spinup &rarr biomass_spinup &rarr final_run```.

During the spinning up of the model, stage "cycling" is often used, in which a pair of stages are cycled through multiple times. To do this, the entry in ```stage_config.yaml``` is prepended by ```multistep```, and then the stages within this cycling process are sub-entries under the ```multistep``` heading. Modifying the previous example to include a stage spinning up the nitrogen and phosphorus pools would like:

```stage_config.yaml```
```yaml
climate_spinup:
    count: 1
biomass_spinup:
    count: 1
multistep_NP_spinup:
    NP_numeric:
        count: 3
    NP_analytic:
        count: 2
final_run:
    count: 1
```

This example would run ```climate_spinup &rarr biomass_spinup &rarr NP_numeric &rarr NP_analytic &rarr NP_numeric &rarr NP_analytic &rarr NP_numeric &rarr final_run```. Note that in the cycled stages, a given stage will be included in the cycling until its count "expires"- if the count of ```NP_numeric``` was set to 5, then the cycle would finish with 3 executions of the ```NP_numeric``` stage. Duplicate stage names are not allowed, stages with ```count: 0``` will be ignored.

### Setting up a Stage

The execution of a CABLE run is defined by a series of namelists. The namelists used in the execution of a given stage are the result of a merge between the "master" namelists, contained in the top level configuration directory, and the "stage" namelists, contained in a directory with the same name as the stage defined in the ```stage_config.yaml```. This means that the master namelist will typically contain the options which remain unchanged across the run, such as the location of ancillary and forcing inputs or write locations, and the stage namelist will contain the binary switches activating/deactivating science modules and the parameters governing them and restart information. Options defined in the stage namelist take precedence over options in the master namelist. If there is no master namelist corresponding to a defined stage namelist, the stage namelist will be taken as is.

Every stage defined in ```stage_config.yaml``` must have an associated namelist directory (even if that directory is empty). Returning to the second example shown above (with the cycled NP stages), if the configuration requires 2 namelists ```cable.nml``` and ```pop.nml```, then the directory structure would look like:

```
demo_configuration/
├── biomass_spinup
│   ├── cable.nml
│   └── pop.nml
├── climate_spinup
│   ├── cable.nml
│   └── pop.nml
├── final_run
│   ├── cable.nml
│   └── pop.nml
├── NP_analytic
│   ├── cable.nml
│   └── pop.nml
├── NP_numeric
│   ├── cable.nml
│   └── pop.nml
├── cable.nml
├── config.yaml
├── pop.nml
└── stage_config.yaml
```

### Handling Restarts

It is necessary to pass restarts, containing the some final state of prior stages, to the following stages. A version of this process already exists within the ```payu``` framework, to make any restarts generated by a given stage available to the next stage. To make use of this, any restarts should be written to the ```restart/``` directory, and prior restarts are available in the top level of the execution directory. So to write a restart, use something like:

```cable.nml```
```
&cablenml
    ...
    filename&restart_out    = "restart/cable_rst.nc"
    ...
/
```

and then to read that restart at the next stage:
```cable.nml```
```
&cablenml
    ...
    filename&restart_in     = "cable_rst.nc"
/
```

There are some CABLE configurations which complicate this process, by requiring restarts from more than just the most recently completed stage. To navigate this issue, after the completion of a given stage, any restarts from the previous stage which are _NOT_ superceded by a restart from the just completed stage are included in the set of restarts passed to the next stage. Returning to the same example used above, if the ```climate_spinup``` stage generates ```restart/cable_rst.nc``` and ```restart/climate_rst.nc``` restarts, and the ```biomass_spinup``` stage generates a ```restart/cable_rst.nc```, then both ```cable_rst.nc``` and ```climate_rst.nc``` will be available to the ```NP_numeric``` stage, with the ```cable_rst.nc``` coming from ```biomass_spinup``` (since it is more recent) and ```climate_rst.nc``` coming from ```climate_spinup``` (there is no newer version). This process only checks by name, not by type of restart.
