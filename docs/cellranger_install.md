---
layout: page
permalink: /cellranger_install/
sidebar: false
---

#### download and set up cellranger on the Charité HPC cluster to map human single cell data

´´´
# move to directory where cellranger is supposed to be located
# cd ??/bin/

# download cellanger
curl -o cellranger-9.0.1.tar.gz "https://cf.10xgenomics.com/releases/cell-exp/cellranger-9.0.1.tar.gz?Expires=1752705465&Key-Pair-Id=APKAI7S6A5RYOXBWRPDA&Signature=QUtKz6UVfppJGXdVnvWqb~HWA9yuCak6IXO42jUcAA~uR~Fb-R~vlYGkK3znh8aNc9jsRD4Y0Iljm5~OBn8PJ2jTwQQ1mardTZF3l1uJtLWEQci9FYEE-9pcHlyuVo1sWM5szLRC8FFzXwH-Wr4syamFPIjIIRexeBbkcKmCqIFSsDtjUcEOmQgOCXLcBNeG891W4KatJjbLCXPeRUndVB6863~DfuD4sDSGAUfgcm04YoPtHheWLs4xw5LzjxkpG~RgrAvFs4hamHquc7T~dyPxwz8Q0qHv~c~H8aHRu~ZA1Nwj8Ie8bB94kIYrh1o2YRWi9S5F20esZpAn30aCOw__"

# unpack cellranger; this takes a bit of time
tar -xvzf cellranger-9.0.1.tar.gz
## -x extract
## -v verbose
## -z use gz algorithm
## -f filename

# test if cellranger can be called
cellranger-9.0.1/cellranger

# help function for cellranger count (which we will later use to generate count matrices)
cellranger-9.0.1/cellranger count --help

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
cellranger-9.0.1/cellranger count --id tutorial --fastqs dataset/fastqs/ --localcores 5 --localmem 50 --output-dir mapped --transcriptome genome/refdata-gex-GRCh38-2024-A/ --create-bam true

´´´
