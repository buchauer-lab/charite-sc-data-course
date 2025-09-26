---
layout: page
permalink: /integrationexercise/
sidebar: false
---

### Dataset Integration Exercise

We will integrate two PBMC datasets from different 10x Chromium chemistry versions to observe and correct batch effects.

### Part 1: Data Acquisition
1. Navigate to the [10x Genomics dataset page](https://www.10xgenomics.com/resources/datasets)
2. Download these two PBMC datasets (Note: you need to enter some information including an email address prior to being able to download data):
  - Dataset 1: [4k PBMCs from v2 chemistry](https://www.10xgenomics.com/datasets/4-k-pbm-cs-from-a-healthy-donor-2-standard-1-2-0)
  - Dataset 2: [5k PBMCs from v4 chemistry](https://www.10xgenomics.com/datasets/5k_Human_Donor3_PBMC_3p_gem-x)
3. Download the "Feature / cell matrix (filtered)" for each dataset

### Part 2: Initial Processing Without Integration

#### Step 1: Load and combine datasets
- Import both datasets into your environment
- Label each dataset with its chemistry version (add metadata column: "batch" or "chemistry")
- Merge/concatenate into a single object

#### Step 2: Standard preprocessing
- Filter cells (min genes, max genes, % mitochondrial)
- Normalize and log-transform
- Find highly variable genes
- Run PCA

#### Step 3: Clustering and visualization
- Compute nearest neighbors
- Run Leiden/Louvain clustering
- Generate UMAP
- Create visualizations:
 - UMAP colored by batch/chemistry version
 - UMAP colored by clusters
 - Feature plots for key PBMC markers: `CD3D` (T cells), `CD14` (Monocytes), `CD79A` (B cells), `NKG7` (NK cells)


### Part 3: Batch Correction

#### For Seurat users:
- Use `IntegrateData()` with default parameters
- Follow the standard Seurat integration workflow

#### For scanpy users:
- Use `scanpy.external.pp.harmony_integrate()`
- Run on the PCA space you computed earlier

### Part 4: Post-Integration Analysis

#### Step 1: Re-process integrated data
- Re-run clustering on integrated data
- Generate new UMAP from integrated embeddings
- Create same visualizations as before:
 - UMAP colored by batch
 - UMAP colored by clusters  
 - Feature plots for the same marker genes

#### Step 2: Quantitative evaluation
Calculate the integration Local Inverse Simpson's Index (iLISI):
- For R users: Use the `lisi` package
 ```r
 # Install: devtools::install_github("immunogenomics/lisi")
 library(lisi)
 res <- compute_lisi(embedding_matrix, metadata_df, "batch")
```

- For python users: Copy the function below and use it to calculate iLISI.
```python
# Install: pip install sklearn
def calculate_lisi(adata, batch_key, embedding_key='X_pca', k=30):
    from sklearn.neighbors import NearestNeighbors
    import numpy as np
    
    # Subsample to smallest batch size
    batch_counts = adata.obs[batch_key].value_counts()
    print(batch_counts)
    min_size = batch_counts.min()
    
    balanced_cells = []
    for batch in batch_counts.index:
        batch_cells = adata.obs[adata.obs[batch_key] == batch].index
        sampled = np.random.choice(batch_cells, min_size, replace=False)
        balanced_cells.extend(sampled)
    
    # Use balanced subset
    adata_subset = adata[balanced_cells]
    X = adata_subset.obsm[embedding_key]
    batch_labels = adata_subset.obs[batch_key].values
    
    nbrs = NearestNeighbors(n_neighbors=k+1).fit(X)
    distances, indices = nbrs.kneighbors(X)
    
    lisi_scores = []
    for i in range(len(X)):
        neighbor_batches = batch_labels[indices[i][1:]]  # exclude self
        unique_batches, counts = np.unique(neighbor_batches, return_counts=True)
        proportions = counts / len(neighbor_batches)
        simpson_index = np.sum(proportions ** 2)
        lisi_scores.append(1 / simpson_index)
    
    return np.mean(np.array(lisi_scores))
```
- iLISI interpretation:
  - Values close to 1 = poor mixing (batch effect remains)
  - Values close to 2 = good mixing (successful integration)

### Part 5: Comparison & Discussion
Discuss with a neighbor:
1. How did cells cluster before vs. after integration?
2. Are cell types now mixed across batches?
3. What is your iLISI score? Does it support your visual assessment?
4. Can you identify the major PBMC cell types after integration?

### Bonus Challenges
- Try different integration parameters (e.g., different numbers of integration features)
- Compare with another integration method if time permits
- Calculate additional metrics like the k-nearest neighbor Batch Effect Test (kBET)