# Spectral neighbor analysis potential 


This repository contains spectral neighbor analysis potential (SNAP) models for 
various elements, including the force field models, the corresponding parameters and training data used to develop the models. For SNAP model training and generate potentials, please use the maml package [https://github.com/materialsvirtuallab/maml](https://github.com/materialsvirtuallab/maml).

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

Below are the units of training metrics:

* Energy: `eV`
* Forces: `eV/Angstrom`
* Stress: `-1 * kbar`

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

The JSON data file contains all the necessary information for developping the force field. Specifically, you will need the descriptors and the targets. The targets are the DFT computed energies per atom in the structure, atomic forces and stress components, which are stored under "data" in the json file. The descriptors for energy, force and stress are different, even though they share the same coefficients. For bispectrum coefficients calculated with $$j_{max}$$ = 3, the energy descriptor is the atomic average of bispectrum coefficients having a dimension of (1, 31) where 31 is a number dictated by the bispectrum coefficient order ($$j_{max}$$), the force descriptor is the first derivatives of individual atomic bispectrum coefficients that has a dimension of (n, 31), where n is the number of atoms, and lastly the stress descriptor is a matrix with dimension (6, 31), where 6 corresponds to the unique six components in the stress tensors.

The model coefficients can be fitted by gathering all data in the training folder and then by a linear model that relates the descriptors to the targets, with the linear fitting weights listed under "optimized params/weights" in the json file. In the previous example, we will need to fit **31** coefficients from the linear model as the SNAP coefficients. 


## Running examples

The examples are under the `usage` folder.
Noting that the current SNAP parameters are compatible with Lammps-2018 version.
For newer version of Lammps, minor changes may need to be adjusted. 

For example, one can run the `Mo/usage/elastic_example` by simply the following command
> lmp_serial -in in.elastic 

The final results should look like the following:

```
=========================================
Components of the Elastic Constant Tensor
=========================================
Elastic Constant C11all = 472.830692948326 GPa
Elastic Constant C22all = 472.830692952315 GPa
Elastic Constant C33all = 472.830692945432 GPa
Elastic Constant C12all = 151.92547560401 GPa
Elastic Constant C13all = 151.925475607608 GPa
Elastic Constant C23all = 151.925475607655 GPa
Elastic Constant C44all = 106.589112603853 GPa
Elastic Constant C55all = 106.589112606229 GPa
Elastic Constant C66all = 106.589112606586 GPa
Elastic Constant C14all = -1.64471728492463e-09 GPa
Elastic Constant C15all = -3.16276324848469e-09 GPa
Elastic Constant C16all = 6.3790059985881e-10 GPa
Elastic Constant C24all = -2.92113212529259e-09 GPa
Elastic Constant C25all = 5.7165004803867e-10 GPa
Elastic Constant C26all = 6.09399861920712e-09 GPa
Elastic Constant C34all = -1.85699578515759e-09 GPa
Elastic Constant C35all = -1.13746544837262e-09 GPa
Elastic Constant C36all = 1.70693557054728e-09 GPa
Elastic Constant C45all = -8.163048780782e-11 GPa
Elastic Constant C46all = 3.04624246101865e-09 GPa
Elastic Constant C56all = 1.08037347240268e-09 GPa
=========================================
Average properties for a cubic crystal
=========================================
Bulk Modulus = 258.89388138718 GPa
Shear Modulus 1 = 106.589112605556 GPa
Shear Modulus 2 = 160.452608671134 GPa
Poisson Ratio = 0.243175631155087
Total wall time: 0:00:06
```
