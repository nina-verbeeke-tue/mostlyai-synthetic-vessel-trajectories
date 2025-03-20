# Synthetic Data Vessel Trajectories using MOSTLY AI

We use the Synthetic Data SDK Python toolkit provided by [MOSTLY AI](https://github.com/mostly-ai/mostlyai) to locally trains and generates synthetic data locally on your own compute resources.

The Synthetic Data SDK allows us to create:

1. **Generators** - Train a synthetic data generator on existing tabular data assets
2. **Synthetic Datasets** - Use a generator to create any number of synthetic samples to your needs

## Overview

This repository utilizes AIS data from Danish waters, obtained from:  _Olesen et al. (2023). AIS Trajectories from Danish Waters for Abnormal Behavior Detection. Technical University of Denmark._  The dataset is available for download [here](https://data.dtu.dk/collections/AIS_Trajectories_from_Danish_Waters_for_Abnormal_Behavior_Detection/6287841).

We perform the following three steps.

| Step                     | Description                                                                                              | Code Notebook |
|--------------------------|----------------------------------------------------------------------------------------------------------|--------------|
| **1. Pre-processing**       | Pre-processing is performed to prepare the data for further steps.                                      | notebook |
| **2. Downsampling**         | To improve synthetic data generation, we reduce the number of position points in the trajectories. A position point is kept only if its distance from the previous point is more than 1 km. | notebook |
| **3. Synthetic Data Generation** | We use the MOSTLY AI toolkit to generate synthetic vessel trajectories.                         | notebook |

<br />

In Step 3, a Generator is created and exported as a ZIP file to the `generator` folder. 
The results of the synthetic vessel trajectory generation are stored in the `synthetic_data` folder.

<br>

# Import Generator

To skip all the previous steps, you can simply import this pre-trained generator and immediately create synthetic vessel trajectories.


```python
from mostlyai.sdk import MostlyAI
import pandas as pd 

# Initialize MOSTLY AI SDK
mostly = MostlyAI(local=True)

# Import the pre-trained generator
g = mostly.generators.import_from_file("generator/1903-denmark-generator.zip")

# Generate synthetic samples
samples = mostly.probe(g, size=1000)

# Extract synthetic positions
syn_positions = samples["positions"]
ps = pd.DataFrame(syn_positions)
ps.to_csv("synthetic_positions.csv", index=False)
```