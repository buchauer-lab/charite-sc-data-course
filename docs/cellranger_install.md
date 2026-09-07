---
layout: page
permalink: /cellranger_install/
sidebar: false
---

#### download and set up cellranger on the Charité HPC cluster to map human single cell data

Get the download link from the 10x Genomics website:

[https://www.10xgenomics.com/support/software/cell-ranger/downloads#download-links](https://www.10xgenomics.com/support/software/cell-ranger/downloads#download-links)
Copy the download command that uses curl. Be aware that file links change frequently.

 ```bash
# move to directory where cellranger is supposed to be located
# cd ??/bin/

# download cellanger using the curl command copied earlier. It should have this structure:
curl -o cellranger-10.1.0.tar.gz "https://cf.10xgenomics.com/releases/cell-exp/cellranger-10.1.0.tar.gz?Expires=1788821212&Key-Pair-Id=APKAI7S6A5RYOXBWRPDA&Signature=F-EvmyBwP5gIhyGwh0-mA7-H7tSqG6Zi5oM~yj6SwUGWACv3JftL0VJGMnVjh5~p6Hj-RKO-lSr25u0Go0WDMUo6Wrdl7yyN21ReyKs71HJvgHK7DEaGLZJYMSDfmTzjrJa1sFS6ezpfaemUxSwSnzV2Eu3K5UJP-vtyeeuDT4LLCHtqzvIox4q7R-z-KwKExGOGdRHZPl84div1MxGCIqQbi~TiURPTyA3YtEGkm-XW3KUeZakwbIoh6yT6QQwudc-N0Fsg3gRKHY6MvhP3K1Z1qmvgfBqqV2M~n7FpU1pVouaDclVwzh6mpMbzt7joULtw5lAd5dswiSEtwafXaw__"

# unpack cellranger; this takes a bit of time
tar -xvzf cellranger-10.1.0.tar.gz
## -x extract
## -v verbose
## -z use gz algorithm
## -f filename

# test if cellranger can be called
cellranger-10.1.0/cellranger

# help function for cellranger count (which we will later use to generate count matrices)
cellranger-10.1.0/cellranger count --help

# move to directory where data is supposed to be stored
# cd ??/data/??

# make folder for reference genome
mkdir genome

# move into genome folder
cd genome

# download human reference genome
curl -O "https://cf.10xgenomics.com/supp/cell-exp/refdata-gex-GRCh38-2024-A.tar.gz"

# unpack reference genome; this takes a bit of time
tar -xvzf refdata-gex-GRCh38-2024-A.tar.gz

# move one folder up again
cd ..

# start cellranger
cellranger-10.1.0/cellranger count --id tutorial --fastqs dataset/fastqs/ --localcores 5 --localmem 50 --output-dir mapped --transcriptome genome/refdata-gex-GRCh38-2024-A/ --create-bam true
```
