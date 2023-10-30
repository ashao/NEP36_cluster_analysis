# Overview
The repository here contains a series of jupyter notebooks that performs a K-means cluster analysis
on data from the benthic layer in the NEP36 model from 1996-2019. These clusters are then used to
establish thresholds for what defines an extreme state.

## Requirements

These notebooks use the following packages:

- scipy
- scikit-learn
- xarray with dask
- matplotlib

Additionally, these notebooks define a variable `datapath` where the raw NEMO model is stored. 
At a minimum the analysis here requires the following variables:

- T
- S
- O2
- DIC
- ALK

## Running the analysis

Many of these notebooks depend on data calculated in a previous notebook, hence if starting
from scratch one will need to run the notebooks in the following order

1. `prepare_datasets_from_raw.ipynb`: This will subset the original files and calculate
   derived quantities like aragonite saturation state and apparent oxygen utilization.
   These data are then saved in the `processed/daily` directory of `datapath`.
2. `calculate_clusters.ipynb`: This will ingest the data from the previous step, calculate
   a climatology, and perform the actual clustering on the dataset. Finally the
   climatological data of each cluster are saved in their own files
3. `separate_daily_data_into_clusters.ipynb`: The notebook takes the definition of the
   clusters the previous step and the processed daily data from step 1 to create
   individual files for each cluster that have daily data, stacked along the x-y
   dimensions
4. `compare_cluster_distributions.ipynb`: Most of the analysis and plots are done here,
   including the calculation of the timeseries, thresholds, compound extremes, and
   durations.