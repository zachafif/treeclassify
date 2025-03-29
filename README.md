# treeclassify
Portfolio project on tree species classification using LiDAR and machine learning

![image](https://github.com/user-attachments/assets/1383d0f9-a582-42c1-9eff-52a63415bb98)


## Introduction

Forest are a crucial renewable natural resource and an important dynamic part of the global carbon cycle.  Periodically, it need to be monitor thoroughly by conducting forest inventory, a systematic collection of data on the forestry resources. One of the most important data to be acquired is Tree species information. Originally, forest inventory are done by manual cruising and insitu survey. However, this approach are expensive and time consuming.

Recent development in the field of forest inventory moved toward the application of remote sensing method such as Light Detection and Ranging (LiDAR) for conducting forest inventory. As LiDAR method works by using pulsed lasers to detect and measure the return from the emitted laser beam that interacted with terrain and object surfaces, it provide range data in the form of three-dimensional point clouds that can penetrate canopy. Thus, it can capture the structure of the forest down to each tree.

![image](https://github.com/user-attachments/assets/94a1fbdf-b211-4e7b-9874-b9c8935ab5fd)


While the acquisition technology can capture detailed data, it need a statistical model in order to generate automatic tree species information. Such modelling can be done using machine learning approaches to correlate tree species with LiDAR data. In this project, we will use random forest algorithm to classify tree species from LiDAR data.

## Related Works
Related works related to this project serve as benchmark for the developed model,

![image](https://github.com/user-attachments/assets/1db01852-bdd4-4024-ac2e-dc2a38a19450)

## Dataset and Features

### Dataset

The dataset, provided by Dubrovin et al. (2024), comprises of point cloud data acquired from a LiDAR sensor mounted on an Unmanned Aerial Vehicle (UAV), complemented by field inventory data that serves as the ground truth reference (label) for the LiDAR measurements. 
Point cloud data is a set of points in a 3D coordinate system with each point contain the following information,

![image](https://github.com/user-attachments/assets/6d7a4397-0814-43d7-b360-3d88827cbb29)

_(Source: ASPRS.org)_

In which usually visualized in 3D view,

![image](https://github.com/user-attachments/assets/ba04aaff-979a-499c-bfd0-3ea35b6b6bf9)

The dataset divided into 10 plots of 100x50 meter with each tree has been labeled with species and diameter on breast height information.

![plot9](https://github.com/user-attachments/assets/75b1e9a4-d58f-470d-9a91-b628eea08cc9)

The study area is situated in Perm Krai, Russia, approximately 90 km east of the regional administrative center, Perm. This region features a dense mixed forest characterized by a complex and irregular canopy structure. LiDAR point cloud already undergo noise cleaning to filter points with extreme values above canopy. After that, ground classification was done to identify points that represent the ground. Finally, the data was normalized by transforming points elevation as height above ground. 

The dataset includes species labels for each of the 3,600 trees surveyed which covers seven species of trees, approximately equally split among coniferous and deciduous: spruce, birch, fir, aspen, tilia, alder, and willow. For this project the main goal is to classify into coniferous and deciduous, therefore each tree species will be assigned to a new column of binary class (coniferous or deciduous).

![image](https://github.com/user-attachments/assets/de00fdf2-638b-4894-859f-c4beaeda602f)

Next, the data also has been checked for Null or NaN values, however turns out there are no both Null or NaN values. 

```
Columns with at least one NaN value: []
```

Then, we try to visualize the each features (columns)  with a kernel density estimate plot to see the distribution. From the plot we can see that the features has varying skewness either negative or positive with several features has near normal bell-shaped appearance.

![image](https://github.com/user-attachments/assets/5a93c53d-4f69-4e6f-89e0-5f3d7b1136fd)

Then, we visualize each feature by class using a boxplot to see the difference of distribution in between classes. From this plot we can see that there are features that has significant difference in between classes and there are some that has little to no difference.

![image](https://github.com/user-attachments/assets/73841153-f87b-4d21-81fa-8885f7cb0088)


### Feature

LiDAR features/metrics were extracted by applying various calculation metrics (descriptive statistics, etc.). Metrics are scalar summaries of point distributions that can be computed using varying neighborhood definitions and varying reference locations. These metrics were calculated based on point height (Z), Intensity (I), and/or return number (R).  For this project, we use a circular plot with radius of three meter field inventory’s tree location data as it center. The result for each plot then will have a singular number of each computed metrics. This example showed us on each tree point has a single value of z_max, metrics that represent the maximum value of LiDAR point height in the plot.

![image](https://github.com/user-attachments/assets/75596b38-4aa1-49d4-b1ff-46627589f544)

_(Source: https://r-lidar.github.io/lidRbook)_


Feautre extraction from LiDAR data were done using R language. For this project, We use all available metrics from R package lidrmetrics. The list of metrics will be provided in a separate excel file with the total of 120 metrics extracted from the data. 


## Method

For this project we will use random forest algorithm as the classifier. Random forest is a machine learning algorithm that made up by multiple decision trees and combined results from each trees to reach a single result. Decision trees is a flowchart-like tree model that make decision based on how a previous set of questions were answered. For a numerical features, the trees will create a threshold value accordingly.

![image](https://github.com/user-attachments/assets/738d582a-510e-46e6-840d-b65644447308)

Another features that also fundamental in random forest is ensemble concept, as random forest done by building multiple decision trees using random samples of the data. First, we select random K data points from the training set. Then, we build the decision trees associated with the selected data points (Subsets). Next, we build N number of decision trees by repeating the first and latter steps. Each tree is trained on a different subset of the data which makes each tree unique and only using several features that chosen randomly.  

![image](https://github.com/user-attachments/assets/128fb944-65dc-431b-88fe-39dcb1eabbb4)

Then after all trees are done, the final outcome will combine all the trees result by majority voting.

![image](https://github.com/user-attachments/assets/e1a2e79a-5aad-4e20-a898-65095d60cce9)

When performing Random Forests based on classification data, we often use the Gini index to decide how nodes on a decision tree branch :

![image](https://github.com/user-attachments/assets/a7814b3e-a921-4c47-b979-10b0d4a0c1c3)

where 𝑝i represents the relative frequency of the class we are observing in the dataset and 𝑐 represents the number of classes. 

Our choice on using random forest is that the algorithm often used to classify LiDAR data by other researchers. As for the reason for the choosing of random forest algorithm are as follows,

1. The algorithm can handles high dimensional data, as LiDAR data derived into several metrics this algorithm can efficiently handle high dimensional data
2. Random Forest is capable of modeling non-linear relationships as LiDAR data contains complex relationships between features
3. The algorithm is quite simple and not require extensive preprocessing and hyperparameter tweaking.

## Experiment, Result, and Discussion

### Variable Selection Result 
To reduce the data dimensionality and to make the model more interpretable, we conduct a variable selection process. We use forward selection approach for our data due to it simplicity and computationally feasible to do with author’s machine. The approach begin with fitting the model using M0 with no input variable,  followed by recursively add all input variable one by one with the cross validation score calculated for each. Next,  the variable with the best score added by one predictor with the same step as before. For this project we chose the best **ten (10)** predictor, this process will yield the top ten predictors with the smallest CV error.

![image](https://github.com/user-attachments/assets/0a3b41ff-8080-46b4-b5b6-47937f01c363)

From the process, we select ten features as follows,

```
'zq15', 'zentropy', 'zpcum8', 'pz_0.0.15', 'pz_5.10', 'rumple', 'kde_peak4_elev', 'kde_peak2_value', 'imean', 'ipcumzq50'
```

With the definition of the ten metrics described in this table,

![image](https://github.com/user-attachments/assets/b7dfb383-df77-4cae-98db-6535ede448b1)

The performance of forward selection features in this project can be shown in the following figure,

![image](https://github.com/user-attachments/assets/89a9ebc6-01f5-4af7-aa85-11f05d21ea47)

The performance shows an improvement as the features are added with a significant increase from two to three features. Then the increasing trend slowly rises to 0.69 before it become stagnant near the end (ten features).

### Hyperparameters
Hyperparameters experimented for this project are,
```
n_estimators=The number of trees in the forest
max_depth=The maximum depth of the tree
min_samples_split=The minimum number of samples required to split an internal node
min_samples_leaf=The minimum number of samples required to be at a leaf node.
```

We test a few different models using GridSearchCV with params_grid set as 
```
{'n_estimators': [100,250,500], 'max_depth': [None, 10, 20], 'min_samples_split': [2, 5, 10], 'min_samples_leaf': [1, 2, 4]}
```

The result can be seen in below figure,

![score per paam](https://github.com/user-attachments/assets/cbbc9897-af5c-4efb-8e08-ebf025c39aef)

and get the best parameter as: 
```
{'max_depth': 10, 'min_samples_leaf': 4, 'min_samples_split': 2, 'n_estimators': 100}
```

### Model Performance
The model performance were evaluated using several performance metric as follows,
```
Accuracy=((TP+TN))/((TP+TN+FP+FN) )     (1)
Precision=((TP))/((TP+FP) )     (2)
Recall=((TP))/((TP+FN) )     (3)
F1-score=2×((precision*recall))/((precision+recall) )     (4)

Where:
TP (True Positives)      : The number of correctly predicted positive instances.
TN (True Negatives)   : The number of correctly predicted negative instances.
FP (False Positives)    : The number of incorrectly predicted positive instances.
FN (False Negatives) : The number of incorrectly predicted negative instances.
```

Accuracy is the most fundamental metrics for classification model evaluation. It measures the proportion of correctly predicted instances ((both true positives and true negatives) among all instances in the dataset. However, accuracy may be misleading in an imbalance dataset, therefore it need additional metrics to support the result such as,
1.	precision, the proportion of true positive among all positives. The metrics showed model ability to make accurate prositive prediction.
2.	Recall, the proportion of true positive among all positives. The metrics showed model ability to correctly identify all positive.
3.	F1-score, balanced assessment between precision and recall
4.	Confusion matrix, graphical representation of all positive and negative instance
5.	Receiver Operating Characteristic (ROC) Curve, graphical representation of a classification model's ability to distinguish between positive and negative classes at various classification thresholds. The curve is plotted from True Positive Rate (recall) as y-axis and False Positive Rate (1-recall) as x-axis. Additionally it also compute Area under Curve (AUC) which is basically evaluate the model’s performance to distinguish positives and negatives.

Furthermore, we divided the data into training set and test set with a ration of 80:20. Then, the training set was used for model fitting, and the training accuracy was evaluated using k-cross validation with k=5. Last, the fitted model were tested against test set to get the final evaluation metrics. For this experiment, we build three scenario model as follows,
1. Scenario #1: Fitted model with all metrics and default hyperparameters
2. Scenario #2:Fitted model with selected metrics and default hyperparameters
3. Scenario #3:Fitted model with selected metrics and selected hyperparameters

The results for these three scenarios shown in below table,

![image](https://github.com/user-attachments/assets/a44a0de2-f976-4cc7-b9b3-2ee548c2024d)

From the comparison table, we can conclude that the selected features with the best parameter has improved overall performance metrics.

Next, the best parameter model yield the following confusion matrix,

![confusion matrix](https://github.com/user-attachments/assets/532e9e53-ef6c-4195-aa95-75f57c585ed2)

The yellow box represent true instances whereas the purple represent false instances. It was shown that the model has true instances twice the false instances, a TPR > TFR which shows a good sign for our model. 
Lastly, we perform ROC curve analysis and yield the following result,

![ROC Curves](https://github.com/user-attachments/assets/b603e444-94ee-4294-891c-0b523acb14d5)

The curve has a AUC of 0.72 which was higher than random guess (0.5). This indicate that the model has better prediction than random guess but it still more improvement as it has a relatively small predictive power.

### Mapping

The predicted result then mapped back into their respective tree using the tree "ID" column, a unique identifier created by concatenating the plot number and tree number. The mapped result then visualized in a GIS software and overlayed in the aerial photo. A sample result also checked in the point cloud level to check the 3D view of the sample trees.

![tree id 2_324](https://github.com/user-attachments/assets/782ba849-c78d-4072-a738-a430ce5bcf12)![tree id 9_303](https://github.com/user-attachments/assets/f31d3c2d-25d6-4404-9231-68207fce66a6)
![tree id 6_220](https://github.com/user-attachments/assets/a684d687-e772-4119-8b56-769eb9db10bb)![tree id 3_48](https://github.com/user-attachments/assets/7985cd6a-30ae-4ea5-a7f3-64345685bf6c)

The full prediction result also plotted,

![Map plot 1](https://github.com/user-attachments/assets/605c0eb0-53f1-4e8d-b6a7-b66bf8d9d947)
![Map plot 9](https://github.com/user-attachments/assets/e6162924-0478-400a-a397-8ec034f1593e)

### Conclusion and Future Works
In this project, we aimed to classify tree species from LiDAR point cloud data using machine learning model. The point cloud data used by dataset provided by Dubrovin et al. (2024) was acquired using LiDAR sensor mounted in a UAV and accompanied with a field inventory data as label reference. The study area was located in Perm, Russia.The output of the project was a binary classification prediction of the tree species, whether it was deciduous or coniferous.  The selected model algorithm for this project was random forest model. Furthermore, we had  experimented on the model feature selection and hyperparameter by using three different scenarios as following, default parameter-all features, default parameter-selected features, best parameters-selected features.

The experiment resulted in only slight differences between scenarios with the accuracy of 0.71-0.74 in the test dataset and F1 score of 0.71-0.74. These results shows that the model still need improvement as it has a relatively small predictive power. Moreover, future research can focus more on improving LiDAR feature extraction technique, better feature selection method, and exploring other model algorithms.

### Reference

Airlangga, G. (2024). Comparative Analysis of Machine Learning Models for Tree Species Classification from UAV LiDAR Data. Buletin Ilmiah Sarjana Teknik Elektro, 6(1), 54–62. https://doi.org/10.12928/biste.v6i1.10059

Dalponte, M., Bruzzone, L., & Gianelle, D. (2012). Tree species classification in the Southern Alps based on the fusion of very high geometrical resolution multispectral/hyperspectral images and LiDAR data. Remote sensing of environment, 123, 258-270.

Dubrovin, I., Fortin, C. & Kedrov, A. An open dataset for individual tree detection in UAV LiDAR point clouds and RGB orthophotos in dense mixed forests. Sci Rep 14, 21938 (2024). https://doi.org/10.1038/s41598-024-72669-5

Jean-Romain Roussel and David Auty (2023). Airborne LiDAR Data Manipulation and Visualization for Forestry Applications. R package version 3.1.0. https://cran.r-project.org/package=lidR

Li, Jili & Hu, Baoxin & Sohn, Gunho & Jing, Linhai. (2010). Individual tree species classification using structure features from high density airborne lidar data. 2099 - 2102. 10.1109/IGARSS.2010.5651629. 

Kim, S., Hinckley, T., & Briggs, D. (2011). Classifying individual tree genera using stepwise cluster analysis based on height and intensity metrics derived from airborne laser scanner data. Remote sensing of environment, 115(12), 3329-3342.

Kim, S., McGaughey, R. J., Andersen, H. E., & Schreuder, G. (2009). Tree species differentiation using intensity data derived from leaf-on and leaf-off airborne laser scanner data. Remote Sensing of Environment, 113(8), 1575-1586.

Marrs, J., & Ni-Meister, W. (2019). Machine Learning Techniques for Tree Species Classification Using Co-Registered LiDAR and Hyperspectral Data. Remote Sensing, 11(7), 819.

Roussel, J.R., Auty, D., Coops, N. C., Tompalski, P., Goodbody, T. R. H., Sánchez Meador, A., Bourdon, J.F., De Boissieu, F., Achim, A. (2021). lidR : An R package for analysis of Airborne Laser Scanning (ALS) data. Remote Sensing of Environment, 251 (August), 112061. <doi:10.1016/j.rse.2020.112061>.

Tarsha-Kurdi, Fayez & Amakhchan, Wijdan & Gharineiat, Zahra. (2021). Random Forest Machine Learning Technique for Automatic Vegetation Detection and Modelling in LiDAR Data. Journal of Environmental Science and Natural Resources. 28. 10.19080/IJESNR.2021.28.556234. 

Qian, C., Yao, C., Ma, H., Xu, J., & Wang, J. (2023). Tree Species Classification Using Airborne LiDAR Data Based on Individual Tree Segmentation and Shape Fitting. Remote Sensing, 15(2), 406.





