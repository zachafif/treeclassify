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

### Dataset

The dataset, provided by Dubrovin et al. (2024), comprises of point cloud data acquired from a LiDAR sensor mounted on an Unmanned Aerial Vehicle (UAV), complemented by field inventory data that serves as the ground truth reference for the LiDAR measurements. The study area is situated in Perm Krai, Russia, approximately 90 km east of the regional administrative center, Perm. This region features a dense mixed forest characterized by a complex and irregular canopy structure. LiDAR point cloud already undergo noise cleaning to filter points with extreme values above canopy. After that, ground classification was done to identify points that represent the ground. Finally, the data was normalized by transforming points elevation as height above ground. 

![image](https://github.com/user-attachments/assets/ba04aaff-979a-499c-bfd0-3ea35b6b6bf9)

The dataset includes species labels for each of the 3,600 trees surveyed which covers seven species of trees, approximately equally split among coniferous and deciduous: spruce, birch, fir, aspen, tilia, alder, and willow. For this project the main goal is to classify into coniferous and deciduous, therefore each tree species will be assigned to a new column of binary class (coniferous or deciduous).

![image](https://github.com/user-attachments/assets/de00fdf2-638b-4894-859f-c4beaeda602f)

Next, the data also has been checked for Null or NaN values, however turns out there are no both Null or NaN values. Then, we try to visualize the each features (columns)  with a kernel density estimate plot to see the distribution. From the plot we can see that the features has varying skewness either negative or positive with several features has near normal bell-shaped appearance.

![image](https://github.com/user-attachments/assets/3adc01c5-330e-4ad6-a385-92c2abb09848)

Then, we visualize each feature by class using a boxplot to see the difference of distribution in between classes. From this plot we can see that there are features that has significant difference in between classes and there are some that has little to no difference.

![image](https://github.com/user-attachments/assets/f63383c1-7ec7-48bf-a0a4-34d3c8e65472)


### Feature

LiDAR features/metrics were extracted by applying various formula (descriptive statistics, etc.) in a circular plot with field inventory’s tree location data as it center. The list of features will be provided in a separate excel file with the total of 120 features extracted from the data. To reduce the data dimensionality and to make the model more interpretable, we conduct a variable selection process. We use forward selection approach for our data due to it simplicity and computationally feasible to do with author’s machine. The approach begin with fitting the model using M0 with no input variable,  followed by recursively add all input variable one by one with the CV score calculated for each. Next,  the variable with the best score added by one predictor with the same step as before. For this project we chose the best ten predictor, this process will yield the top ten predictors with the smallest CV error.

![image](https://github.com/user-attachments/assets/0a3b41ff-8080-46b4-b5b6-47937f01c363)

From the process, we select ten features as follows,

_'pzabove2', 'CRR', 'zpcum8', 'L1', 'pz_5.10', 'n_first', 'Oligophotic', 'kde_peak1_value', 'imax', 'ipcumzq30'_

## Method

For this project we will use random forest algorithm as the classifier. Random forest is a machine learning algorithm that made up by multiple decision trees and combined results from each trees to reach a single result. Decision trees is a flowchart-like tree model that make decision based on how a previous set of questions were answered. For a numerical features, the trees will create a threshold value accordingly.

![image](https://github.com/user-attachments/assets/738d582a-510e-46e6-840d-b65644447308)

Another features that also fundamental in random forest is ensemble concept. As random forest done by building multiple decision trees using random samples of the data. Each tree is trained on a different subset of the data which makes each tree unique and only using several features that chosen randomly.  

![image](https://github.com/user-attachments/assets/128fb944-65dc-431b-88fe-39dcb1eabbb4)

Then after all trees are done, the final outcome will combine all the trees result by majority voting.

![image](https://github.com/user-attachments/assets/e1a2e79a-5aad-4e20-a898-65095d60cce9)

Our choice on using random forest is that the algorithm often used to classify LiDAR data by other researchers. As for the reason for the choosing of random forest algorithm are as follows,

1. The algorithm can handles high dimensional data, as LiDAR data derived into several metrics this algorithm can efficiently handle high dimensional data
2. Random Forest is capable of modeling non-linear relationships as LiDAR data contains complex relationships between features
3. The algorithm is quite simple and not require extensive preprocessing and hyperparameter tweaking.

## Experiment, Result, and Discussion

### Variable Selection Result 
The variable selection resulted in the top ten best features to be used for modelling. This feature are 

### Hypermarameters
Hyperparameters experimented for this project 

### Model Performance

### Conclusion and Future Works




