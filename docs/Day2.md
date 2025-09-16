---
layout: page
title: Day 2
order: 3
---

### Day 2

#### Morning
- _Lecture_: [What is a count matrix and how is it represented in Seurat/Scanpy?](https://github.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/L_count_matrix.pdf)
- _Lecture_: [Quality Control for scRNA-Seq data](https://github.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/L_quality_control.pdf)
- _Practical work_: Importing the scRNA-Seq example dataset and performing first QC steps
  - [Download the example dataset from the course github](https://github.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/healthy_PBMCs.zip). This is a subset of the 10x Genomics v3 10k PBMC dataset (“10k PBMCs from a Healthy Donor (v3 chemistry)”; https://www.10xgenomics.com/datasets/10-k-pbm-cs-from-a-healthy-donor-v-3-chemistry-3-standard-3-0-0). Cell numbers were reduced while keeping the individual cells’ sequencing depth the same. Only some cell types in the original dataset were kept. A number of empty droplets and low-quality cells are also included.
  - [basic scRNA-Seq workflow exercises - python](https://github.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/basic_sc_workflow_python.ipynb)
  - basic scRNA-Seq workflow exercises - R

#### Afternoon
- _Lecture_: [Data preprocessing](https://github.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/L_preprocessing.pdf)
- _Lecture_: [Dimensionality reduction 1 - Principal Component Analysis](https://github.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/L_dimred_PCA.pdf)
- _Practical work_: Data preprocessing (comparison of standard log norm approach and Pearson residuals) and PCA, preliminary cell type analysis
  - (uses the same notebooks as the morning exercise, see above)

---
layout: default
title: "Lecture 1 - Introduction to single-cell transcriptomics"
---

<div style="text-align: center; margin: 20px 0;">
    <h1>{{ page.title }}</h1>
    
    <embed src="https://raw.githubusercontent.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/L_count_matrix.pdf" 
           type="application/pdf" 
           width="100%" 
           height="800px"
           style="border: 1px solid #ccc;">
    
    <p style="margin-top: 10px;">
        <a href="https://raw.githubusercontent.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/L_count_matrix.pdf" target="_blank">
            Open PDF in new tab
        </a>
    </p>
</div>
