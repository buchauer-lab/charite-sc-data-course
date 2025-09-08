---
layout: page
permalink: /doubletexercise/
sidebar: false
---

## Doublet Detection Exercise

Here, we will explore how to identify and remove doublets (two cells captured in one droplet) from single-cell RNA-seq data using computational methods.

### Dataset
We will work with the 10x Genomics 10k PBMCs from a Healthy Donor (v3 chemistry). The higher cell count increases doublet probability (~8% expected doublet rate).
- Download link: [10k PBMCs, 3' v3.1](https://www.10xgenomics.com/resources/datasets/10k-human-pbmcs-3-ht-v3-1-chromium-x-3-1-high)
- Download the "Feature / cell matrix (filtered)" 

### Part 1: Initial Data Processing

#### Step 1: Load and explore the dataset
- Import the 10k PBMC dataset
- Check initial cell and gene counts
- Note: With ~10,000 cells loaded, expect 600-800 doublets

#### Step 2: Standard preprocessing
- Calculate QC metrics (% mitochondrial, nGenes, nUMIs)
- Create QC plots:
 - Scatter plot: nUMIs vs nGenes (color by % mitochondrial)
 - Histogram of nUMIs per cell

#### Step 3: Process without doublet removal
- Filter cells using standard thresholds
- Normalize, find variable genes, scale
- Run PCA → UMAP → clustering
- Save this object as "pre_doublet_removal"

### Part 2: Doublet Detection

#### For Seurat users:
Install [scDblFinder](https://bioconductor.org/packages/release/bioc/html/scDblFinder.html) and use it according to the package documentation.

#### For scanpy users:
Use `scrublet` via the built-in scanpy wrapper:
 - `scanpy.external.pp.scrublet(adata, expected_doublet_rate=0.08)`

#### Step 4: Analyze doublet predictions
- Add doublet scores/classifications to your object
- Create visualizations:
 - UMAP colored by doublet score (continuous)
 - UMAP colored by doublet classification (binary)
 - Violin plot of nUMIs split by singlet/doublet
 - Histogram of doublet scores with threshold line

### Part 3: Biological Validation

#### Step 1: Check for heterotypic doublets
Look for cells expressing mutually exclusive markers:
- T cell + B cell markers: `CD3D` + `CD79A`
- T cell + Monocyte markers: `CD3D` + `CD14`
- B cell + NK markers: `CD79A` + `NKG7`

Create feature plots:
- Plot these marker pairs
- Overlay predicted doublets


#### Step 2: Examine clustering patterns
- Which clusters have the highest proportion of predicted doublets?
- Do any clusters consist mainly of doublets?
- Calculate doublet percentage per cluster

### Part 4: Remove Doublets and Re-analyze

#### Step 1: Filter doublets
- Remove cells classified as doublets
- Document how many cells were removed

#### Step 2: Re-process cleaned data
- Re-run the full pipeline (normalize → HVG → PCA → UMAP → cluster)
- Save this object as "post_doublet_removal"

#### Step 3: Compare before and after
Create side-by-side comparisons:
1. Number of cells retained
2. UMAP plots (before/after)
3. Number of clusters
4. Marker gene expression clarity

### Part 5: Critical Evaluation

Discuss with a neighbor:
1. What percentage of cells were identified as doublets?
2. Were doublets randomly distributed or concentrated in specific areas?
3. Did doublet removal reveal any new cell populations?
4. Did any clusters disappear after doublet removal?
5. How did doublet scores correlate with QC metrics (nUMIs, nGenes)?

