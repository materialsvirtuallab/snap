# Using the training data

The training data is provided in json files, which can be read by any standard json parser. Each json file contains contains one data group (e.g., AIMD-NpT, AIMD-NVT, elastic, surfaces, etc.) used in the training of the SNAP model.

The data is provided as a list of dicts. Each dict contains one structure with data. The format is as follows:

```
[
    {
        "structure": Dict representation of Structure as output by pymatgen,
        "data": {
            "energy_per_atom": -1.24324,
            "forces": [],
            "virial_stress": float
        },
        "optimized_bispectrum_coefficients": [],
        "optimized_weight": float
    },
    ...
]
```