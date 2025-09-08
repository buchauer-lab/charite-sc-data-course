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

- For python users: Use the `scib` package
```python
# Install: pip install scib
import scib
ilisi = scib.metrics.ilisi_graph(adata, batch_key="batch", type_="embed")
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