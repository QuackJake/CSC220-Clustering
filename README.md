# Wine Dataset Clustering Analysis

## Project Overview

This project analyzes the underlying structure of the **Wine dataset** from `sklearn.datasets` using multiple unsupervised clustering algorithms. The goal is to compare how different clustering approaches identify groups within the wine samples and determine whether those groups represent meaningful differences in chemical composition.

The analysis compares five clustering methods:

* **K-Means**
* **K-Medoids**
* **Agglomerative Clustering**
* **Gaussian Mixture Models (GMM)**
* **DBSCAN**

The project includes exploratory data analysis, feature scaling, PCA dimensionality reduction, model selection, cluster evaluation, visualization, and stability analysis.

---

## Dataset

The analysis uses the `load_wine()` dataset from `sklearn.datasets`.

* **Samples:** 178 wine observations
* **Features:** 13 chemical properties
* **Target:** Three wine classes, used only as a reference and **not** during clustering

The chemical features describe properties such as alcohol, flavonoids, phenolic compounds, color intensity, and other measurements associated with the wines.

---

## Preprocessing

Before clustering, the data was prepared using the following process:

1. **Feature Scaling**
   All 13 chemical features were standardized using `StandardScaler` so that features with larger numerical ranges did not disproportionately influence the clustering algorithms.

2. **Dimensionality Reduction**
   Principal Component Analysis (PCA) was applied to reduce the dimensionality of the standardized dataset while retaining approximately **95% of the variance**.

3. **Outlier Diagnostics**
   An IQR-based outlier analysis was performed to identify potentially unusual observations. The identified observations were not automatically removed from the dataset.

PCA also provided a useful representation for visualizing the resulting clusters and examining how the major sources of variation contribute to separation between groups.

---

## Clustering Methods

### 1. K-Means

K-Means partitions observations into a predefined number of clusters by minimizing within-cluster variance.

The analysis uses:

* Elbow method / WCSS to evaluate candidate cluster counts
* Silhouette scores for cluster quality
* Cluster centers in PCA space
* 2D PCA cluster visualizations
* Silhouette plots

K-Means provided a useful baseline because the PCA-transformed data is well suited to centroid-based clustering.

### 2. K-Medoids

K-Medoids is similar to K-Means but uses actual observations as cluster representatives rather than calculated centroids.

The analysis includes:

* Multiple random initializations
* Within-cluster distance
* Silhouette scores
* Gap statistic
* Medoid locations
* Cluster stability analysis
* PCA cluster visualizations

K-Medoids was particularly useful for evaluating whether the identified structure remained consistent across different initializations.

### 3. DBSCAN

DBSCAN identifies dense regions of observations rather than requiring a predetermined number of clusters.

The analysis explores:

* `eps`
* `min_samples`
* Number of discovered clusters
* Number of observations classified as noise
* Silhouette score for non-noise observations
* k-distance analysis
* Parameter sensitivity
* PCA cluster visualizations

DBSCAN produced the highest silhouette score among the tested approaches, but this result required additional consideration because **more than 100 of the 178 observations were classified as noise**. As a result, the high silhouette score does not necessarily indicate that DBSCAN was the most useful model for this dataset.

### 4. Agglomerative Clustering

Agglomerative clustering builds a hierarchy of clusters by progressively combining observations.

The analysis compares different linkage strategies, including:

* Ward
* Complete
* Average
* Single where applicable

Cluster quality was evaluated using silhouette scores, with dendrograms and PCA visualizations used to examine the resulting structure.

### 5. Gaussian Mixture Models

Gaussian Mixture Models treat the data as a mixture of underlying probability distributions rather than assigning observations strictly to geometric cluster boundaries.

The analysis evaluates multiple covariance structures:

* Full
* Tied
* Diagonal
* Spherical

Model selection uses:

* **BIC**
* **AIC**
* Silhouette scores
* PCA visualizations
* Gaussian component ellipses

GMM produced slightly better silhouette results than several of the other practical clustering approaches, although the resulting grouping was broadly consistent with the other models.

---

## Model Comparison

The different algorithms produced broadly consistent views of the underlying structure, but each method emphasized different characteristics of the data.

| Model             | Key Strength                                 | Key Limitation                                     |
| ----------------- | -------------------------------------------- | -------------------------------------------------- |
| **K-Means**       | Simple and effective centroid-based baseline | Assumes relatively compact, spherical clusters     |
| **K-Medoids**     | Robust representatives and stable clustering | More computationally expensive than K-Means        |
| **Agglomerative** | Reveals hierarchical relationships           | Results depend on linkage strategy                 |
| **GMM**           | Models probabilistic cluster membership      | Assumes a Gaussian mixture structure               |
| **DBSCAN**        | Detects dense regions and outliers           | Classified a large portion of the dataset as noise |

K-Medoids and Agglomerative Clustering produced intuitive solutions with approximately **three clusters**, while GMM achieved slightly stronger silhouette performance without substantially changing the overall grouping.

DBSCAN produced the highest silhouette score, but more than 100 observations were labeled as noise. This makes the result less practical for this dataset despite its strong numerical score.

Overall, no algorithm completely contradicted the others. Instead, the comparison demonstrated that the models capture different aspects of the same underlying structure.

---

## Cluster Quality Assessment

The resulting clusters appear to represent meaningful differences within the Wine dataset.

Several forms of evidence support this conclusion:

* Clear separation is visible in PCA space.
* Cluster profiles show differences across the original chemical features.
* Statistical testing indicates significant differences between clusters.
* Boxplots and radar charts reveal distinct chemical profiles.
* Effect sizes provide additional evidence that the differences are not merely statistically significant but potentially meaningful.
* K-Medoids produced consistent cluster assignments across different initializations.

The dataset contains only **178 observations**, which makes the relatively well-defined cluster structure particularly useful to examine, while also limiting how confidently the results can be generalized to larger wine populations.

The agreement between multiple clustering approaches, visualizations, statistical analysis, and stability testing provides stronger evidence that the observed groupings reflect meaningful structure rather than random variation or overfitting.

---

## Feature Analysis

PCA indicates that much of the separation between observations is driven by chemical characteristics associated with:

* Alcohol
* Flavonoids
* Color intensity
* Phenolic compounds

**PC1** provided the strongest contribution to the observed separation, followed by **PC2**. These components capture important chemical variation within the dataset and help explain why the major wine groupings are relatively distinct.

The later PCA dimensions contribute progressively less to the overall variance. For clustering purposes, the first several components therefore provide a more compact representation of the dataset while retaining most of its information.

Future experiments could investigate whether removing low-variance dimensions or incorporating additional chemically relevant features produces even clearer cluster separation.

---

## Visualizations

The project includes several visualizations for evaluating the clustering results, including:

* PCA cluster maps
* Silhouette plots
* Elbow/WCSS plots
* K-Medoids convergence plots
* Gap statistic analysis
* DBSCAN k-distance plots
* DBSCAN parameter sensitivity heatmaps
* Agglomerative dendrograms
* GMM BIC/AIC plots
* Gaussian component visualizations
* Cluster feature boxplots
* Radar charts
* Cross-model cluster comparisons

A universal visualization utility is also used to compare the resulting cluster assignments across the different algorithms.

---

## Practical Applications

Although this analysis is performed on a relatively small academic dataset, the approach demonstrates how clustering can be applied to wine chemistry and product analysis.

Potential applications include:

* **Wine research:** Identifying groups of wines with similar chemical profiles.
* **Winemaking:** Investigating relationships between chemical composition, grape selection, and fermentation processes.
* **Product differentiation:** Identifying distinct chemical profiles that could correspond to different product characteristics.
* **Marketing and pricing:** Using objectively measured characteristics to investigate potential product segments.
* **Quality analysis:** Comparing new samples against previously identified chemical groupings.

With a larger and more detailed dataset, this type of clustering pipeline could potentially be incorporated into a winery or wine research workflow for continuously evaluating new samples.

---

## Limitations & Future Work

### Limitations

The primary limitation is that clustering was performed using a PCA-transformed representation of the original data. While PCA preserves most of the dataset's variance, the resulting components do not directly correspond to individual original chemical features.

Other limitations include:

* The dataset contains only 178 observations.
* Outliers can still influence cluster boundaries.
* DBSCAN's treatment of many observations as noise demonstrates the sensitivity of density-based methods to this dataset.
* Clustering results depend on the selected preprocessing and model parameters.
* The analysis is based on a specific set of chemical measurements and may not generalize to other wine populations.

### Future Work

Potential extensions include:

* Ensemble clustering
* Semi-supervised clustering when partial labels are available
* Deep clustering methods
* Nonlinear dimensionality reduction
* Additional chemical measurements
* Larger datasets with more wine samples
* More granular chemical and environmental features
* Longitudinal analysis as new samples are collected

A larger production dataset could also allow the clustering pipeline to be continuously updated as new wines are introduced, making it possible to monitor how new samples relate to previously identified chemical profiles.

---

## Ethical Considerations

The clustering performed in this project is primarily based on objective chemical measurements, so direct privacy concerns are minimal for the original dataset.

However, the potential applications become more complicated if chemical clustering is combined with external information such as customer demographics, purchasing behavior, or geographic markets.

For example, organizations could potentially use customer segmentation alongside wine characteristics to optimize pricing, distribution, or marketing strategies. While segmentation itself is not inherently unethical, combining datasets can introduce concerns around transparency, discrimination, and how the resulting classifications are used.

If this type of system were expanded into a real-world winery or research environment, proprietary information would also need to be considered. Detailed chemical data could reveal information about production processes, crop management, or other characteristics that a winery may consider commercially sensitive.

Responsible use would therefore require clear documentation of:

* What data is being collected
* How clusters are generated
* What the clusters represent
* How the results are used
* Who has access to the underlying data

---

## Repository Structure

| File / Folder | Description |
|---|---|
│ `EDA.ipynb` | Exploratory data analysis |
│ `ClusteringModels.ipynb` | Clustering model development, tuning, and comparison |
│ `figures/` │ Generated analysis visualizations |
│ `ClusteringModelsAnalysis.pdf` | Project/lab takeaways |
│ `requirements.txt` | Python dependencies |

---

## Technologies & Libraries

This project uses Python and the following primary tools and libraries:

* **Python**
* **Jupyter Notebook**
* **NumPy**
* **Pandas**
* **Scikit-learn**
* **Matplotlib**
* **Seaborn**
* **SciPy**

---

## Running the Project

Clone the repository and install the required dependencies:

```bash
git clone https://github.com/QuackJake/cluster-model-analysis.git
cd cluster-model-analysis
pip install -r requirements.txt
```

Then open the notebooks using Jupyter:

```bash
jupyter notebook
```

The exploratory analysis can be found in `EDA.ipynb`, while the clustering analysis and model comparison can be found in `ClusteringModels.ipynb`.

---

## Summary

This project demonstrates how multiple clustering algorithms can be used to investigate the structure of a real-world chemical dataset.

The results show that the Wine dataset contains a relatively strong and consistent clustering structure. While DBSCAN achieved the highest silhouette score, its classification of more than half of the dataset as noise made it less practical than the other approaches. K-Medoids, Agglomerative Clustering, K-Means, and GMM produced more useful representations of the overall dataset structure.

The agreement between clustering algorithms, PCA visualizations, statistical testing, feature analysis, and stability testing suggests that the identified groups represent meaningful differences in wine chemistry rather than artifacts of a single modeling technique.
