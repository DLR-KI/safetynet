<!--
SPDX-FileCopyrightText: 2024 German Aerospace Center (DLR) <https://dlr.de>

SPDX-License-Identifier: MIT
-->

# SafetyNet Manifest

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/DLR-KI/safetynet/main.svg)](https://results.pre-commit.ci/latest/github/DLR-KI/safetynet/main)
[![REUSE status](https://api.reuse.software/badge/github.com/DLR-KI/safetynet)](https://api.reuse.software/info/github.com/DLR-KI/safetynet)

This directory contains the JSON schema for the different components of the SafetyNet standard.

## Usage

This repository contains the JSON schema to verify an arbitrary JSON file against the SafteyNet standard.
The current version of the SafetyNet standard is `1.0.0`.
Accordingly, the JSON schema are located in the `1.0.0` directory and the raw urls are:

- Manifest: <https://raw.githubusercontent.com/DLR-KI/safetynet/refs/heads/main/1.0.0/manifest.schema.json>
- NNet: <https://raw.githubusercontent.com/DLR-KI/safetynet/refs/heads/main/1.0.0/nnet.schema.json>
- SafetyNet: <https://raw.githubusercontent.com/DLR-KI/safetynet/refs/heads/main/1.0.0/safetynet.schema.json>

For a more detailed description of the SafetyNet standard, please refer to the paper.

## Business Logic

Unfortunately, the JSON schema does not support the full range of business logic that is required to validate the SafetyNet standard.
For example, cross checks between files are not possible.
Therefore, to fully validate the SafetyNet standard, a custom validator is required.
This custom validator should adhere to the following rules:

| ID    | Name                   | Description                                                                                                                                                                                                                                                                                                                      |
| ----- | ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| G-001 | Available Files        | It shall be ensured that all files referenced in the manifest are available.                                                                                                                                                                                                                                                     |
| G-002 | Datatype Coherence     | It shall be ensured that the same datatype is used for all neural networks and lookup tables referred to in the manifest. This datatype shall be ensured to be the same as the datatype specified in the manifest. It shall moreover be used for both the input and output vector of both the neural networks and lookup tables. |
| G-003 | Input Coherence        | It shall be ensured that all neural networks and lookup tables adhere to the structure of the inputs defined in the manifest. This includes but is not limited to ensuring that all input strides defined by the neural networks and lookup tables adhere to the input strides defined by the manifest.                          |
| G-004 | Input Coverage         | It shall be ensured that all the inputs of all neural networks and lookup tables cover at least the required input space defined in the conditionals of the manifest.                                                                                                                                                            |
| G-005 | Output Number          | It shall be ensured that all neural networks and lookup tables adhere to the number of outputs defined in the manifest.                                                                                                                                                                                                          |
| G-006 | Output Type            | It shall be ensured that the output of both the neural networks and lookup tables are an arbitrary vector of numbers with the length ensured by G-005.                                                                                                                                                                           |
| G-007 | Ensured Responsibility | It shall be ensured that every allowed input vector is covered by at least one neural network or lookup table.                                                                                                                                                                                                                   |
| G-008 | Single Responsibility  | It shall be ensured that every allowed input vector is covered by at most one neural network and lookup table.                                                                                                                                                                                                                   |
| G-009 | Condition Limits       | It shall be ensured that all conditions of the neural networks and lookup tables are within the limits defined in the input range in the manifest of the corresponding neural network or lookup table.                                                                                                                           |
| G-010 | Wildcard Conditional   | It shall be ensured that all inputs which are not part of the conditions are not part of the decision process.                                                                                                                                                                                                                   |
| M-001 | Versioning             | It shall be ensured that the version of the manifest adheres to the version of the schema.                                                                                                                                                                                                                                       |
| M-002 | Compatible Versioning  | It shall be ensured that the version of the neural networks and lookup tables are compatible with the manifest.                                                                                                                                                                                                                  |
| L-001 | Correct Output         | It shall be ensured that the output of the lookup table has the correct length and datatype for all defined outputs.                                                                                                                                                                                                             |
| L-002 | Known Format           | It shall be ensured that all lookup tables are in an allowed format.                                                                                                                                                                                                                                                             |
| L-003 | Relayed Responsibility | It shall be ensured that the lookup table can determine whether the responsibility for any given valid input vector lies within the lookup table or if the neural network has to be queried.                                                                                                                                     |
| N-001 | Correct Output         | It shall be ensured that the output of the neural network as defined in the manifest has the correct length and datatype for all defined outputs.                                                                                                                                                                                |
| N-002 | Known Format           | It shall be ensured that all neural networks are in an allowed format.                                                                                                                                                                                                                                                           |

## Related Software

The SafetyNet standard has been implemented in the following software:

- [Advisory Viewer](https://aeronautical-informatics.github.io/openCAS/)
- [openCAS](https://github.com/aeronautical-informatics/openCAS)

## Citation

If you found our work useful, please cite our paper:

```text
@InProceedings{Christensen2024,
    author    = {Christensen, Johann Maximilian and Zaeske, Wanja and Anilkumar Girija, Akshay and Friedrich, Sven and Stefani, Thomas and Durak, Umut and K{\"{o}}ster, Frank and Kr{\"{u}}ger, Thomas and Hallerbach, Sven},
    booktitle = {2024 {IEEE/AIAA} 43st Digital Avionics Systems Conference ({DASC})},
    date      = {2024-09},
    title     = {Towards Certifiable AI in Aviation: A Framework for Neural Network Assurance Using Advanced Visualization and Safety Nets},
    publisher = {{IEEE}},
}
```
