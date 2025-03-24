# Synthetic Data Vessel Trajectories using MOSTLY AI

This repository provides instructions for training your own synthetic vessel trajectory generator. However, if you prefer to skip the training process, you can import the pre-trained generator instead. The pre-trained generator is available [here](https://github.com/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/tree/main/generator), allowing you to directly create synthetic vessel trajectories. 

You can find the code to import the pre-trained generator and generate synthetic data [here](https://github.com/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/blob/main/notebooks/example/importing_pretrained_mostlyai_generator.ipynb), or click the button below to open it in Google Colab.

<a target="_blank" href="https://colab.research.google.com/github/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/blob/main/notebooks/example/importing_pretrained_mostlyai_generator.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

<br />


# Create a generator

We use the Synthetic Data SDK Python toolkit provided by [MOSTLY AI](https://github.com/mostly-ai/mostlyai) to locally trains and generates synthetic data locally on your own compute resources.

The Synthetic Data SDK allows us to create:

1. **Generators** - Train a synthetic data generator on existing tabular data assets
2. **Synthetic Datasets** - Use a generator to create any number of synthetic samples to your needs

<br />

# Overview

We perform the following three steps:

| Step                     | Description                                                                                              | Code |
|--------------------------|----------------------------------------------------------------------------------------------------------|--------------|
| **1. Pre-processing**       | Pre-processing is performed to prepare the data for further steps.                                      | [Code](https://github.com/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/blob/main/notebooks/1_pre_process.ipynb) |
| **2. Downsampling**         | To improve synthetic data generation, we reduce the number of position points in the trajectories. A position point is kept only if its distance from the previous point is more than 1 km. | [Code](https://github.com/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/blob/main/notebooks/2_downsampling.ipynb) |
| **3. Synthetic Data Generation** | We use the MOSTLY AI toolkit to generate synthetic vessel trajectories.                         | [Code](https://github.com/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/blob/main/notebooks/3_mostly_ai.ipynb) |

<br />

After running Step 3, it produces the following two outputs:

* **Generator**: In Step 3, a Generator is created and exported as a ZIP file to the `generator` folder, that can be found [here](https://github.com/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/tree/main/generator). 
* **Synthetic data**: The results of the synthetic vessel trajectory generation are stored in the `synthetic_data` folder, that can be found [here](https://github.com/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/tree/main/synthetic_data). This file can, for example, be imported into [kepler.gl](https://kepler.gl/) for visualizing the results.

<br />

# Input Dataset

This repository uses AIS data from Danish waters, obtained from:  _Olesen et al. (2023). AIS Trajectories from Danish Waters for Abnormal Behavior Detection. Technical University of Denmark._  The dataset is available [here](https://data.dtu.dk/collections/AIS_Trajectories_from_Danish_Waters_for_Abnormal_Behavior_Detection/6287841).

<br />

# Run the code
We specifically used the following file `data_AIS_Custom_01062021_30112021_CarFisHigMilPasPleSaiTan_600_99999999_0.pkl` that can be downloaded by clicking here: https://data.dtu.dk/ndownloader/files/38129544

If you want to re-run all the steps, you should download this file and place it in the `input_data` folder with the same filename.

Alternatively, if you only want to run Step 3, we have uploaded the files `denmark_positions_downsampled.csv` and `denmark_trips_downsampled.csv` in the input_data folder, which are the results of running Step 1 (Pre-processing) and Step 2 (Downsampling). These files are used as inputs for Step 3 (Synthetic Data Generation).

<br />

# Import Generator

To skip all the previous steps, you can simply import this pre-trained generator, that can be found [here](https://github.com/nina-verbeeke-tue/mostlyai-synthetic-vessel-trajectories/tree/main/generator), and immediately create synthetic vessel trajectories.


```python
from mostlyai.sdk import MostlyAI
import pandas as pd 

# Initialize MOSTLY AI SDK
mostly = MostlyAI(local=True)

# Import the pre-trained generator
g = mostly.generators.import_from_file("1903-denmark-generator.zip")

# Generate synthetic samples
samples = mostly.probe(g, size=1000)

# Extract synthetic positions
syn_positions = samples["positions"]
ps = pd.DataFrame(syn_positions)
ps.to_csv("synthetic_positions.csv", index=False)
```