# Wine Dataset Clustering Analysis

A machine learning project comparing multiple unsupervised clustering algorithms on the Wine dataset from scikit-learn, including exploratory data analysis, dimensionality reduction, cluster evaluation, visualization, and stability analysis.

## Overview

This project analyzes the underlying structure of the Wine dataset using five clustering approaches — K-Means, K-Medoids, Agglomerative Clustering, Gaussian Mixture Models (GMM), and DBSCAN. The goal is to compare how each algorithm identifies groups within the wine samples and to determine whether those groups represent meaningful differences in chemical composition. The workflow moves through exploratory data analysis, feature scaling, PCA dimensionality reduction, model selection, cluster evaluation, visualization, and cluster stability testing.

## Repository Structure

| File / Folder                  | Description                                          |
| ------------------------------ | ---------------------------------------------------- |
| `EDA.ipynb`                    | Exploratory data analysis of the Wine dataset        |
| `ClusteringModels.ipynb`       | Clustering model development, tuning, and comparison |
| `figures/`                     | Generated analysis visualizations                    |
| `ClusteringModelsAnalysis.pdf` | Project/lab takeaways                                |
| `requirements.txt`             | Python dependencies                                  |

## Dataset

Uses the built-in Wine dataset from scikit-learn (`sklearn.datasets.load_wine`):

* **Samples**: 178 wine observations
* **Features**: 13 chemical properties (alcohol, flavonoids, phenolic compounds, color intensity, and other measurements)
* **Target**: Three wine classes, used only as a reference for evaluation and never during clustering

## Preprocessing

* **Feature Scaling** — all 13 chemical features were standardized with `StandardScaler` so that features with larger numerical ranges didn't disproportionately drive the clustering.
* **Dimensionality Reduction** — PCA was applied to the standardized data, retaining approximately 95% of the variance. The PCA space is also used to visualize clusters and examine how the major sources of variation separate the groups.
* **Outlier Diagnostics** — an IQR-based analysis flagged potentially unusual observations; these were not automatically removed from the dataset.

## Methods Implemented

1. **K-Means** — centroid-based partitioning, evaluated with the elbow method/WCSS, silhouette scores, cluster centers, PCA visualizations, and silhouette plots. Served as the baseline, since the PCA-transformed data suits centroid-based clustering well.
2. **K-Medoids** — similar to K-Means but uses actual observations as cluster representatives. Evaluated across multiple random initializations, within-cluster distance, silhouette scores, the gap statistic, medoid locations, stability analysis, and PCA visualizations. Particularly useful for checking whether the discovered structure held up across initializations.
3. **Agglomerative Clustering** — builds a hierarchy of clusters via progressive merging. Compared Ward, Complete, Average, and Single linkage strategies where applicable, with quality assessed via silhouette scores, dendrograms, and PCA visualizations.
4. **Gaussian Mixture Models (GMM)** — models the data as a mixture of probability distributions rather than hard geometric boundaries. Evaluated Full, Tied, Diagonal, and Spherical covariance structures using BIC, AIC, silhouette scores, PCA visualizations, and Gaussian component ellipses.
5. **DBSCAN** — density-based clustering that doesn't require a predetermined cluster count. Tuned via `eps` and `min_samples`, and evaluated on cluster count, noise points, silhouette score (non-noise observations), k-distance analysis, parameter sensitivity, and PCA visualizations.

## Usage

1. Clone the repository and install dependencies:

   ```
   git clone https://github.com/QuackJake/cluster-model-analysis.git
   cd cluster-model-analysis
   pip install -r requirements.txt
   ```
2. Launch Jupyter: `jupyter notebook`
3. Run `EDA.ipynb` first for exploratory analysis, then `ClusteringModels.ipynb` for clustering, tuning, and model comparison
4. Review the generated visualizations in `figures/`

## Results

### Model Comparison

The five algorithms produced broadly consistent views of the data's structure, though each emphasized different characteristics of it.

| Model         | Key Strength                              | Key Limitation                                     |
| ------------- | ----------------------------------------- | -------------------------------------------------- |
| K-Means       | Simple, effective centroid-based baseline | Assumes relatively compact, spherical clusters     |
| K-Medoids     | Robust representatives, stable clustering | More computationally expensive than K-Means        |
| Agglomerative | Reveals hierarchical relationships        | Results depend on linkage strategy                 |
| GMM           | Models probabilistic cluster membership   | Assumes a Gaussian mixture structure               |
| DBSCAN        | Detects dense regions and outliers        | Classified a large portion of the dataset as noise |

K-Medoids and Agglomerative Clustering both converged on an intuitive ~3-cluster solution, while GMM achieved slightly stronger silhouette performance without meaningfully changing the overall grouping. DBSCAN produced the *highest* silhouette score of any method, but more than 100 of the 178 observations were classified as noise — making that score less representative of a genuinely useful clustering for this dataset despite the strong number. No algorithm contradicted another; instead, the comparison shows the models capturing different aspects of the same underlying structure.

### Cluster Quality Assessment

Several lines of evidence suggest the resulting clusters reflect meaningful differences within the Wine dataset rather than random variation:

* Clear separation is visible in PCA space.
* Cluster profiles differ across the original chemical features.
* Statistical testing shows significant differences between clusters.
* Boxplots and radar charts reveal distinct chemical profiles per cluster.
* Effect sizes suggest the differences are meaningful, not just statistically significant.
* K-Medoids produced consistent cluster assignments across different initializations.
* Cross-model comparisons show broad agreement in the underlying group structure.

With only 178 observations, the relatively well-defined structure is useful to examine, but it also limits how confidently these results can be generalized to a broader wine population.

### Feature Analysis

PCA indicates that much of the separation between wine samples is driven by chemical characteristics tied to **alcohol, flavonoids, color intensity, and phenolic compounds**. PC1 contributes most to the observed separation, followed by PC2, with later components contributing progressively less variance. For clustering purposes, the first several components provide a compact representation that retains most of the dataset's information. Future work could explore whether dropping low-variance dimensions, or adding further chemically relevant features, sharpens cluster separation even more.

### Practical Applications

Though this analysis uses a relatively small academic dataset, the pipeline demonstrates how clustering could apply to wine chemistry and product analysis more broadly:

* **Wine research** — identifying groups of wines with similar chemical profiles
* **Winemaking** — investigating relationships between chemical composition, grape selection, and fermentation
* **Product differentiation** — surfacing distinct chemical profiles tied to different product characteristics
* **Marketing and pricing** — using objective chemical measurements to explore potential product segments
* **Quality analysis** — comparing new samples against previously identified chemical groupings

With a larger, more detailed dataset, this pipeline could plausibly be incorporated into a winery or research workflow for continuously evaluating new samples.

### Limitations & Future Work

**Limitations**

* Clustering was performed on a PCA-transformed representation, so the resulting components don't map directly back to individual original chemical features.
* The dataset contains only 178 observations.
* Outliers can still influence cluster boundaries even without being removed.
* DBSCAN's high noise classification shows how sensitive density-based methods are to this dataset.
* Results depend on the chosen preprocessing steps and model parameters, and may not generalize to other wine populations.
* The analysis is based on a specific set of chemical measurements and may not capture other factors that could influence wine groupings.

**Future Work**

* Ensemble clustering
* Semi-supervised clustering when partial labels are available
* Deep clustering methods and nonlinear dimensionality reduction
* Additional chemical measurements and larger datasets with more samples
* More granular chemical and environmental features
* Longitudinal analysis as new samples are collected, potentially enabling continuous monitoring of how new wines relate to previously identified chemical profiles
* Additional cross-model comparison techniques to investigate agreement between clustering solutions

### Ethical Considerations

The clustering here relies on objective chemical measurements, so direct privacy concerns with the original dataset are minimal. Risk increases if chemical clustering were combined with external information such as customer demographics, purchasing behavior, or geographic markets — segmentation itself isn't inherently unethical, but combining datasets like this raises concerns around transparency, discrimination, and how resulting classifications get used. Any real-world deployment would also need to account for commercially sensitive information a winery might not want disclosed, including production processes, crop management, or other proprietary characteristics. Responsible use would call for clear documentation of what data is collected, how clusters are generated, what the clusters represent, how results are used, and who has access to the underlying data.

## Technologies & Libraries

* Python
* Jupyter Notebook
* NumPy
* Pandas
* Scikit-learn
* Matplotlib
* Seaborn
* SciPy

## Summary

This project demonstrates how multiple clustering algorithms can be used to investigate the structure of a real-world chemical dataset. The Wine dataset shows a relatively strong and consistent clustering structure: while DBSCAN achieved the highest silhouette score, classifying more than half the dataset as noise made it less practical than the alternatives. K-Medoids, Agglomerative Clustering, K-Means, and GMM all produced more useful representations of the overall structure. The agreement across clustering algorithms, PCA visualizations, statistical testing, feature analysis, stability testing, and cross-model comparisons suggests the identified groups reflect meaningful differences in wine chemistry rather than artifacts of any single modeling technique.

## License

This project uses the Wine dataset from scikit-learn, which is in the public domain.
