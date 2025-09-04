---
layout: page
title: Day 2
order: 3
---

### Day 2

#### Morning
- _Lecture_: What is a count matrix and how is it represented in Seurat/Scanpy? __TODO link to slide deck__
- _Lecture_: Quality Control for scRNA-Seq data __TODO link to slide deck__
- _Practical work_: Importing the example dataset and performing first QC steps _
  - [Download the example dataset from the course github](https://github.com/buchauer-lab/charite-sc-data-course/blob/main/materials/Day2/healthy_PBMCs.zip). This is a subset from the 10x Genomics v3 10k PBMC dataset (“10k PBMCs from a Healthy Donor (v3 chemistry)”; https://www.10xgenomics.com/datasets/10-k-pbm-cs-from-a-healthy-donor-v-3-chemistry-3-standard-3-0-0). Cell numbers were reduced while keeping the individual cells’ sequencing depth the same. Only some cell types in the original dataset were kept. A number of empty droplets and low-quality cells are also included.
  - basic workflow exercises - python
  - basic workflow exercises - R

#### Afternoon
- _Lecture_: Data preprocessing steps and principal component analysis 
- _Practical work_: Data preprocessing (comparison of standard log norm approach and Pearson residuals) and PCA, preliminary cell type analysis
  - (uses the same notebooks as the morning exercise)
