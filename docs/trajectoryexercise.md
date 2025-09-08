---
layout: page
permalink: /trajectoryexercise/
sidebar: false
---

## Trajectory Inference Exercise

### Objective
Learn to infer and analyze cellular differentiation trajectories using pseudotime analysis to understand developmental processes and cell fate decisions.

### Dataset
10x Genomics Mouse Bone Marrow Mononuclear Cells
- Download link: [Human Bone Marrow Mononuclear Cells](https://www.10xgenomics.com/datasets/10k-bone-marrow-mononuclear-cells-bmmncs-5-v2-0-without-intronic-reads-2-standard)
- Download the "Feature / cell matrix (filtered)"
- This dataset contains hematopoietic progenitors and cells at various differentiation stages with clear developmental trajectories

### Part 1: Data Preparation

#### Step 1: Load and preprocess
- Import the bone marrow dataset
- Perform standard QC filtering (min genes, max genes, % mitochondrial)
- Normalize and log-transform
- Find highly variable genes
- Scale data
- Run PCA (use 30-50 components)

#### Step 2: Initial clustering and visualization
- Compute neighbors (k=30)
- Run Leiden/Louvain clustering
- Generate UMAP
- Identify major lineages using key markers:
 - Myeloid: `LYZ`, `CSF1R`, `CD14`, `CD68`
 - Erythroid: `GATA1`, `KLF1`, `HBB`, `GYPA`
 - B cells: `CD79A`, `CD79B`, `MS4A1`, `CD19`
 - T cells: `CD3D`, `CD3E`, `CD4`, `CD8A`
 - Stem/Progenitors: `CD34`, `KIT`, `PROM1`, `THY1`

#### Step 3: Subset data for trajectory analysis
- Focus on myeloid differentiation (monocyte/macrophage lineage)
- Select clusters expressing `LYZ` and early markers like `KIT`
- Re-run PCA on subset (20-30 components)
- Re-compute neighbors and UMAP

### Part 2: Trajectory Inference

#### For Python/scanpy users (Diffusion Pseudotime):

Compute diffusion map and pseudotime:
- Run `sc.tl.diffmap(adata_subset)` to compute diffusion map
- Identify root cell with high `CD34`/`KIT` and low `LYZ`/`CD14`
- Set root: `adata.uns['iroot'] = root_cell_index`
- Compute DPT: `sc.tl.dpt(adata_subset)`
- Visualize: `sc.pl.umap(adata_subset, color=['dpt_pseudotime'])`

#### For R/Seurat users (Monocle3):

Install and load required packages:
- Install: `BiocManager::install('monocle3')`
- Install: `remotes::install_github('satijalab/seurat-wrappers')`
- Load libraries: `library(SeuratWrappers)` and `library(monocle3)`

Convert and prepare data:
- Convert to Monocle3: `cds <- as.cell_data_set(seurat_subset)`
- Preprocess: `cds <- preprocess_cds(cds, num_dim = 30)`
- Reduce dimensions: `cds <- reduce_dimension(cds)`
- Cluster cells: `cds <- cluster_cells(cds)`
- Learn trajectory: `cds <- learn_graph(cds)`

### Part 3: Ordering Cells in Pseudotime

#### Step 1: Select root cells
Identify starting point of trajectory:
- Look for cells with high stem/progenitor markers (`CD34`, `KIT`, `PROM1`)
- Low expression of differentiated markers (`LYZ`, `CD14`, `CD68`)

For Python/scanpy users:
- Find cells with high `CD34` expression: check with `sc.pl.umap(adata, color='Kit')`
- Identify appropriate root cell index
- Set root: `adata.uns['iroot'] = selected_index`

For R/Monocle3 users:
- Interactive selection: `cds <- order_cells(cds)` (opens plot for clicking)
- Or programmatic: identify high CD34 cells and use `order_cells(cds, root_cells = high_kit_cells)`

#### Step 2: Visualize pseudotime
Create visualizations:
- UMAP colored by pseudotime
- UMAP colored by cluster
- Pseudotime distribution histogram
- Original markers overlaid on trajectory

### Part 4: Gene Dynamics Along Pseudotime

Select key genes and create visualizations:
- **Early markers** (should decrease): `CD34`, `KIT`, `PROM1`, `FLT3`
- **Mid markers** (should peak midway): `CEBPA`, `SPI1` (PU.1), `MPO`
- **Late markers** (should increase): `LYZ`, `CD14`, `CD68`, `CSF1R`, `ITGAM` (CD11b)

Depending on your method/environment, check what other options for visualizations are available and try them out.

### Part 5: Biological Interpretation

#### Validate trajectory makes biological sense:
1. Check marker progression:
  - Do stem markers decrease along pseudotime?
  - Do differentiation markers increase?
  - Are intermediate states present?

2. Compare with known biology:
  - Does trajectory match known myeloid development?
  - Are transcription factors expressed at expected times?

3. Discuss to what extent it makes sense in systems like this to divide cells into discrete populations. What are the advantages and disadvantages?



