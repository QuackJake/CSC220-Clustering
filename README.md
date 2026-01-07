# Wine Dataset Clustering Analysis

## Project Overview
This project performs an extensive clustering analysis on the **Wine dataset** from `sklearn.datasets`. Multiple clustering algorithms are applied and evaluated, including:

- **KMeans**
- **K-Medoids**
- **DBSCAN**
- **Agglomerative Clustering**
- **Gaussian Mixture Models (GMM)**

The analysis includes preprocessing, dimensionality reduction, model selection, and visualization to provide a comprehensive understanding of the underlying structure of the data.

---

## Dataset
- **Dataset Used:** `sklearn.datasets.load_wine()`
- **Features:** 13 chemical properties of wines.
- **Target:** Wine class (used only for reference, not for clustering).

---

## Preprocessing
1. **Scaling:** All features are standardized using `StandardScaler`.
2. **Dimensionality Reduction:** PCA is applied to retain **95% of variance**, reducing the data to `n_components`.
3. **Outlier Diagnostics:** Optional outlier detection via IQR method (no removal applied).

---

## Clustering Methods

### 1. KMeans
- **Optimal K Determination:** Uses Elbow Method (inertia/WCSS) and silhouette scores.
- **Outputs:**
  - Optimal number of clusters
  - Cluster labels
  - Per-cluster silhouette scores
  - Cluster centers in PCA space
- **Visualization:** 2D scatter plot of PCA1 vs PCA2, silhouette plot.

### 2. K-Medoids
- **Distance Matrix Approach:** Uses a custom K-Medoids implementation with multiple random initializations for stability.
- **Evaluation Metrics:** Total within-cluster distance (inertia), silhouette scores, Gap Statistic.
- **Visualization:** Inertia convergence, silhouette scores, 2D cluster scatter, medoid positions.

### 3. DBSCAN
- **Parameter Tuning:** Epsilon (`eps`) and minimum samples (`min_samples`) are explored via a grid search.
- **Evaluation Metrics:** Number of clusters, number of noise points, silhouette score for non-noise points.
- **Visualization:** k-distance plot, sensitivity heatmap, 2D cluster scatter showing core and border points.

### 4. Agglomerative Clustering
- **Linkage Methods:** `ward`, `complete`, `average` (optional `single`).
- **Evaluation Metrics:** Silhouette score for cluster selection.
- **Visualization:** Dendrogram, silhouette by number of clusters, 2D cluster scatter.

### 5. Gaussian Mixture Models (GMM)
- **Covariance Types:** `full`, `tied`, `diag`, `spherical`.
- **Evaluation Metrics:** BIC and AIC to select the optimal number of components.
- **Visualization:** BIC/AIC plots, 2D scatter with cluster ellipses representing Gaussian components.

---

## Universal Visualization
A utility function combines all clustering results for side-by-side visualization:
- 2D scatter plots with cluster assignments.
- Approximate decision boundaries for each clustering method.

