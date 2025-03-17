# treeclassify
Portfolio project on tree species classification using LiDAR and machine learning

## Introduction

Forest are a crucial renewable natural resource and an important dynamic part of the global carbon cycle.  Periodically, it need to be monitor thoroughly by conducting forest inventory, a systematic collection of data on the forestry resources. One of the most important data to be acquired is Tree species information. Originally, forest inventory are done by manual cruising and insitu survey. However, this approach are expensive and time consuming. 
Recent development in the field of forest inventory moved toward the application of remote sensing method such as Light Detection and Ranging (LiDAR) for conducting forest inventory. As LiDAR method works by using pulsed lasers to detect and measure terrain and object surfaces, it provide range data in the form of three-dimensional point clouds that can penetrate canopy. Thus, it can capture the structure of the forest down to each tree.
While the acquisition technology can capture detailed data, it need a statistical model in order to generate automatic tree species information. Such modelling can be done using machine learning approaches to correlate tree species with LiDAR data. In this project, we will use random forest algorithm to classify tree species from LiDAR data.

## Related Works
Related works related to this project serve as benchmark for the developed model,
![image](https://github.com/user-attachments/assets/e00f445a-e673-4060-a0d0-e6e4995f80d9)

## Dataset and Features

The dataset, provided by Dubrovin et al. (2024), comprises point cloud data acquired from a LiDAR sensor mounted on an Unmanned Aerial Vehicle (UAV), complemented by field inventory data that serves as the ground truth reference for the LiDAR measurements. The study area is situated in Perm Krai, Russia, approximately 90 km east of the regional administrative center, Perm. This region features a dense mixed forest characterized by a complex and irregular canopy structure. The dataset includes species labels for each of the 3,600 trees surveyed which covers seven species of trees, approximately equally split among coniferous and deciduous: spruce, birch, fir, aspen, tilia, alder, and willow.

LiDAR point cloud already undergo noise cleaning to filter points with extreme values above canopy. After that, ground classification was done to identify points that represent the ground. Finally, the data was normalized by transforming points elevation as height above ground. Finally, the LiDAR features/metrics were extracted by applying various formula (descriptive statistics, etc.) in a circular plot with field inventory’s tree location data as it center.

