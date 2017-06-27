# Using the training data

The training data is provided in json files, which can be read by any standard json parser. Each json file contains contains one data group (e.g., AIMD-NpT, AIMD-NVT, elastic, surfaces, etc.) used in the training of the SNAP model.

The data is provided as a list of dicts. Each dict contains one structure with data. The format of the json file is as follows:

```json
[
    {
        "structure": {"Dict representation of Structure as output by pymatgen"},
        "group": "AIMD-NVT",
        "description": "Snapshot 1 of 40 of AIMD NVT simulation at 300 K",
        "data": {
            "energy_per_atom": -1.24324,
            "forces": [],
            "virial_stress": []
        },
        "optimized_params": {
            "bispectrum_coefficients": [],
            "weight": 1000
        }
    },
    ...
]
```

# Data sources

* Surfaces with Miller indices up to 3.

    ```
    Tran, R.; Xu, Z.; Radhakrishnan, B.; Winston, D.; Sun, W.; Persson, K. A.; Ong, S. P. Surface energies of elemental crystals. Sci. Data 2016, 3, 160080 DOI: 10.1038/sdata.2016.80.
    ```

* (110) Σ3 twist, (111) Σ3 tilt, (110) Σ11 twist, (100) Σ5 twist and (310) Σ5 tilt grain boundaries

    ```
    Tran, R.; Xu, Z.; Zhou, N.; Radhakrishnan, B.; Luo, J.; Ong, S. P. Computational study of metallic dopant segregation and embrittlement at Molybdenum grain boundaries. Acta Mater. 2016, 117, 91–99 DOI: 10.1016/j.actamat.2016.07.005.
    ```
