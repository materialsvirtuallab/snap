# Spectral neighbor analysis potential 


This repository contains spectral neighbor analysis potential (SNAP) models for 
various elements, including the force field models, the corresponding parameters and training data used to develop the models. 

The structure of the folder organization is as follows

```
- Element/
    - training/
    - usage/
        - elastic_example/
    - element.snapcoeff
    - element.snapparam
```
where the `training` folder contains the original datasets used for developing the model, `element.snapcoeff` and `element.snapparam` are the force field parameters ready for LAMMPS, and `usage` contains examples for running the simulations with developed potentials. 


## LAMMPS installation 
The calculations rely on [LAMMPS package](http://lammps.sandia.gov/). During installation, the SNAP package should be installed by the following command in the `src` directory. 
> make yes-snap

You can check if the inclusion is successful by 
> make package-status

We recommand the users to install also the `REPLICA` , `MEAM` and `PYTHON`packages following the same procedure. 

After that, the serial version of lammps can be installed by 
> make serial 

Please refer to the [lammps website](http://lammps.sandia.gov/) for the installation of other versions.

In case the user wants to use the `lammps` package in python (not supported by python 3 for now), lammps should be installed as a shared library by the following command.
> make serial mode=shlib

Usually, the lammps simulations are run by 
> lmp_serial -in foo.in

assuming the executable is `lmp_serial` and the input file is `foo.in`.


## Potential development

The JSON data file contains all the necessary information for developping the force field. Specifically, you will need the descriptors and the targets. The targets are the DFT computed energies per atom in the structure, atomic forces and stress components, which are stored under "data" in the json file. The descriptors for energy, force and stress are different, even though they share the same coefficients. For bispectrum coefficients calculated with $$j_{max}$$ = 3, the energy descriptor is the atomic average of bispectrum coefficients having a dimension of (1, 31), the force descriptor is the first derivatives of individual atomic bispectrum coefficients that has a dimension of (n, 31), where n is the number of atoms, and lastly the stress descriptor is a matrix with dimension (6, 31), where 6 corresponds to the unique six components in the stress tensors.

The model coefficients can be fitted by gathering all data in the folder and then by a linear model that relates the descriptors to the targets, with the linear fitting weights listed under "optimized params/weights" in the json file. 


## Running examples

The examples are under the `usage` folder. 

For example, one can run the `Mo/usage/elastic_example` by simply the following command
> lmp_serial -in in.elastic 

The final results should look like the following:

```
=========================================
Components of the Elastic Constant Tensor
=========================================
Elastic Constant C11all = 472.74677443108 GPa
Elastic Constant C22all = 472.746774452395 GPa
Elastic Constant C33all = 472.746774442108 GPa
Elastic Constant C12all = 151.6255440472 GPa
Elastic Constant C13all = 151.625544047475 GPa
Elastic Constant C23all = 151.625544050317 GPa
Elastic Constant C44all = 106.300142462772 GPa
Elastic Constant C55all = 106.300142457006 GPa
Elastic Constant C66all = 106.300142455713 GPa
Elastic Constant C14all = -1.3836227846884e-09 GPa
Elastic Constant C15all = 1.45629074398045e-09 GPa
Elastic Constant C16all = 2.84505145804073e-09 GPa
Elastic Constant C24all = -2.59511946590807e-09 GPa
Elastic Constant C25all = -1.44417840298876e-09 GPa
Elastic Constant C26all = -1.8974384224466e-10 GPa
Elastic Constant C34all = -5.06655308923479e-09 GPa
Elastic Constant C35all = 1.95395196356453e-09 GPa
Elastic Constant C36all = 3.99011759849483e-09 GPa
Elastic Constant C45all = 2.84064660331229e-09 GPa
Elastic Constant C46all = 3.17902853719524e-09 GPa
Elastic Constant C56all = 6.75295833345557e-10 GPa
=========================================
Average properties for a cubic crystal
=========================================
Bulk Modulus = 258.665954179508 GPa
Shear Modulus 1 = 106.300142458497 GPa
Shear Modulus 2 = 160.560615196765 GPa
Poisson Ratio = 0.242844757139426
Total wall time: 0:00:07

```


