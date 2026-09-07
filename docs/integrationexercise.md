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
- Filter cells (min genes, max genes, % mitochondrial)
- Merge/concatenate into a single object
  - Note: the two datasets were produced with different Cell Ranger reference versions, so their gene lists do not match exactly. Keep only the genes common to both. In scanpy, `sc.concat([a, b], join='inner')` does this automatically; in Seurat, `merge()` keeps the union of genes, so subset to the shared genes (e.g. via `intersect()` of the two feature sets) before or after merging.

#### Step 2: Standard preprocessing
- Normalize and log-transform
- Find highly variable genes
- Run PCA

#### Step 3: Clustering and visualization
- Compute nearest neighbors
- Run Leiden/Louvain clustering (scanpy users: use `sc.tl.leiden(adata, flavor="igraph", n_iterations=2)`, matching the Day 2 workflow)
- Generate UMAP
- Create visualizations:
 - UMAP colored by batch/chemistry version
 - UMAP colored by clusters
 - Feature plots for key PBMC markers: `CD3D` (T cells), `CD14` (Monocytes), `CD79A` (B cells), `NKG7` (NK cells)


### Part 3: Batch Correction

#### For Seurat users (Seurat v5):
In Seurat v5, the two chemistry versions live as separate `counts` layers inside the RNA assay (this happens automatically when you `merge()` the two objects; you can also enforce it with `obj[["RNA"]] <- split(obj[["RNA"]], f = obj$batch)`). Integration then operates directly on this single, layered object - there is no longer a separate `IntegrateData()` step as in Seurat v4.
- Make sure you have run the standard preprocessing from Part 2 (`NormalizeData`, `FindVariableFeatures`, `ScaleData`, `RunPCA`) on the merged, layered object
- Integrate the layers with `IntegrateLayers()`, using the PCA you already computed as input:
 ```r
 obj <- IntegrateLayers(
   object = obj,
   method = HarmonyIntegration,   # or CCAIntegration / RPCAIntegration
   orig.reduction = "pca",
   new.reduction = "harmony"
 )
 # after integration, rejoin the split layers for all downstream steps
 obj <- JoinLayers(obj)
 ```
- `HarmonyIntegration` requires the `harmony` package (`install.packages("harmony")`). Using it here keeps the R and python workflows comparable, since the python side below also uses Harmony. `CCAIntegration` is the classic Seurat default if you prefer.
- In Part 4, point `FindNeighbors()` and `RunUMAP()` at the integrated embedding via `reduction = "harmony"` (or whatever you passed to `new.reduction`) instead of `"pca"`.

#### For scanpy users:
- Run Harmony on the PCA space you computed earlier:
 ```python
 import scanpy.external as sce
 sce.pp.harmony_integrate(adata, key='batch')   # 'batch' = your batch column in adata.obs
 ```
- This reads `adata.obsm['X_pca']` and writes the corrected embedding to `adata.obsm['X_pca_harmony']`. It does **not** rebuild your neighbor graph automatically.
- **This is the step that is easy to get wrong:** you have to tell the downstream functions to use the corrected embedding, otherwise nothing actually changes. In Part 4, build the neighbor graph on the Harmony embedding:
 ```python
 sc.pp.neighbors(adata, use_rep='X_pca_harmony')
 sc.tl.umap(adata)
 sc.tl.leiden(adata, flavor='igraph', n_iterations=2)
 ```
- `harmony_integrate` needs the `harmonypy` package. If it is not already in your environment, install it once with `pip install harmonypy` (or add `harmonypy` to your mamba environment).
- A few parameters are worth knowing (all optional, passed straight through to `harmonypy`). Start with the defaults and only tune if the UMAP shows a problem:
  - `max_iter_harmony` (default 10): maximum number of Harmony iterations. If the run reports that it did not converge, raise this (e.g. 20-30).
  - `theta` (default 2): how strongly batches are pushed to mix. Higher values force stronger mixing but can begin to blend genuinely distinct cell types; lower values are gentler. This is the main knob to reach for if you see over- or under-correction.
  - For reproducible results, set `np.random.seed(0)` before the call.

### Part 4: Post-Integration Analysis
- Re-run clustering and UMAP on the integrated embedding (see the language-specific notes above for which reduction / representation to use)
- Generate the same visualizations as in Part 2:
 - UMAP colored by batch
 - UMAP colored by clusters
 - Feature plots for the same marker genes

### Part 5: Comparison & Discussion
Discuss with a neighbor:
1. How did cells cluster before vs. after integration?
2. Are cell types now mixed across batches?
3. Judging from the UMAPs, was the integration successful? Are there populations that stayed separated by batch, or any that now look over-mixed?
4. Can you identify the major PBMC cell types after integration?

### Bonus Challenges
- Try a different integration method or setting and compare (e.g. Harmony vs. CCA in Seurat, or a different `theta` in scanpy)
- Check how well a rarer population (e.g. NK cells, `NKG7`) is preserved rather than washed out by the correction