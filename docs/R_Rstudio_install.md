---
layout: page
permalink: /R_Rstudio_install/
sidebar: false
---


#### Instructions to install R and Rstudio to your computer (local installation)

#### Step 1a - Download R and RStudio if you don't have them installed already:

If you donʼt have R on your computer -> [Download R (preferably version 4)](https://cran.r-project.org/)

If you donʼt have RStudio on your computer -> [Download RStudio](https://www.rstudio.com/products/rstudio/download/)

Install first R and then RStudio, following the instructions on the respective websites.


#### Step 1b - If you have R and RStudio installed already:
Check your R version by opening RStudio

![](/images/R_version_screen_cropped.png)


#### Step 2a - If you still have R version 3, follow these instructions:

*Instructions for R version 4 can be found in the next section.*

You should think about updating your R version at this stage. But note that this will deprecate all packages that you have previously installed, meaning you need to re-install them. Packages may be a bit different between versions or not available for R version 4 – if you have scripts based on R version 3 and depend on them, it may be better to stick to R version 3 for now. Please keep in mind that the class may use newer Seurat commands, which may not be available to you, so continue at your own risk.

Load Seurat version 3 - In your console in RStudio, execute the following commands:

```
install.packages("remotes")
remotes::install_version("Seurat", version = "3.2.3")
```

Install a couple of other packages that you will need by executing the commands below. This includes the umap-learn functionality, which requires python. If you don't have python on your computer, RStudio will likely ask you whether you want to install a minimal python environment via miniconda. If you get asked this question, enter y and press return.

```
install.packages('ggplot2')
reticulate::py_install(packages = 'umap-learn')
install.packages('reshape2')
install.packages('dplyr')
```

You can load the installed packages with the "library()" command to see whether everything is working properly:

```
library(Seurat)
library(reshape2)
library(ggplot2)
library(dplyr)
```

If (almost) nothing happens, when executing these commands, everything is fine! You've successfully prepared for the upcoming course.

#### Step 2b - If you have R version 4, follow these instructions:

```
install.packages('Seurat')
install.packages('reshape2')
install.packages('ggplot2')
install.packages('dplyr')
```

If you see the warning message below, enter y and press return:

```
package which is only available in source form, and may need compilation
of C/C++/Fortran: 'Seurat'
Do you want to attempt to install these from sources?
y/n:
```

You can load the installed packages with the "library()" command to see whether everything is working properly:

```
library(Seurat)
library(reshape2)
library(ggplot2)
library(dplyr)
```

If (almost) nothing happens, when executing these commands, everything is fine! You've successfully prepared for the upcoming course.
