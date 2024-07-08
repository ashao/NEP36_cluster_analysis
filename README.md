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
4. `compare_cluster_distributions.ipynb`: Most of the analysis and many of the plots are done here,
   including the calculation of the timeseries, thresholds, compound extremes, and
   durations. For the fraction of extremes plots, we output pickle files and then plot them in the next notebook for        efficiency.
5. `compare_cluster_distributions_other_clusters.ipynb`: The fraction of extremes is computed for the remaining clusters (supp figures)
6. `plots_frac_extreme`: Using the pickles computed in 4, this notebook makes the publication quality plots. 
7. `prepare_depth_comparison`: This notebook extracts regions with depths less than 77m for comparison with the shaloows cluster. 
8. `compare_depth_distributions`: run after `separate_daily_data_into_clusters.ipynb` and `prepare_depth_comparison`. This notebook compares the clustering approach for dividing up the domain to one that uses a simpler depth delineation (supplementary figs). 
9. `prepare_MLD_fromraw.ipynb`: extracts the MLD for each cluster and saves as an h5 file
10. `compute_cluster_extreme_fraction`: This notebook computes the fraction of extremes as in 4, but saves them differently for the correlation analysis.
11. `correlations_fracextreme_climateindices.ipynb`: this notebook compute the Pearson correlation along with the confidence interval from the pickles created in `compute_cluster_extreme_fraction`. 