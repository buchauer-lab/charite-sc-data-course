---
layout: page
permalink: /installpython/
sidebar: false
---

#### Instructions for installing python and the single-cell data analysis kit scanpy to your laptop (local installation)

#### Step 1 - install miniforge3

We will first install `miniforge` which is itself an installer for `mamba` which is an installer for python packages (kind of like a kinase kinase kinase).
- Go to [https://github.com/conda-forge/miniforge](https://github.com/conda-forge/miniforge) and scroll down to see the install instructions for your system
- For Windows, install from the executable installer with default settings
- For macOS, you need to open the application "Terminal", e.g. via the Launchpad, and enter the command starting with `curl` followed by the command starting with `bash`. Please ask an instructor for help if you are using this workflow.

To verify `miniforge` is installed correctly:
- Open a Terminal:
	- If on Windows, open the Software "Miniforge Prompt" which we have just installed
	- If on MacOS / Linux, open a regular terminal
- Type `conda --version` and press Enter
- You should see a version number displayed
- On MacOS, type `conda init zsh` to initialize your shells for `conda`/`mamba` use.


#### Step 2 - create an environment for your data analysis

The package installers `mamba` and `conda` allow you to create separate analysis environments into which you can install required software for data analysis. The advantage of this approach is that different projects, which might require different versions of the same software package, are kept separate and cannot interfere negatively with one another. Here, we use `mamba` for package installation, because it is faster.
- Open a terminal (as described in Step 1)
- type each of the following lines and hit enter.
	- `conda create -n scrnaseq python=3.10`(this creates an analysis environment called `scrnaseq`)
    - when prompted, type 'y' and press Enter to proceed
	- `conda activate scrnaseq`(this activates the environment, which should become evident by the environment name now preceding the prompt)
- Install core packages required for our course
 ```bash
mamba install -c conda-forge -c bioconda scanpy pandas numpy matplotlib seaborn ipykernel scikit-learn scipy h5py openpyxl leidenalg python-igraph -y
```
This does not work if you are behind Charité's proxy server. "Charité Gast" WiFi and eduroam (without using Charité VPN!) should work.

Not all packages are available via `conda`/`mamba`, so sometimes we need to use the more general python package manager `pip`. Here, we use it to install `decoupler`, which comes with many useful functions for single cell analysis, and `PyDESeq2` for differential expression testing. Please run the following commands inside your `scrnaseq` environment:

```bash
pip install decoupler
pip install pydeseq2
```

#### Step 3 - install Positron

This year we will use [Positron](https://positron.posit.co/) as our code editor. Positron is a data science IDE (from the makers of RStudio) that works with both python and R and can run jupyter notebooks directly, without a web browser.
- Go to [https://positron.posit.co/download.html](https://positron.posit.co/download.html) and download the installer for your operating system
- Install with the default settings
- Open Positron once to make sure it starts correctly

#### Step 4 - connect Positron to your environment and start coding

Because we created the `scrnaseq` environment in Step 2 *before* opening Positron, Positron will automatically find it.
- In Positron, open (or create) a new notebook file (ending in `.ipynb`) via `File > New File > Jupyter Notebook`
- Select the python interpreter for this notebook: click the interpreter/kernel selector in the top right of the notebook (or open the Command Palette with `Ctrl`/`Cmd` + `Shift` + `P` and search for "Select Interpreter"), and choose the `scrnaseq` conda environment from the list
	- If `scrnaseq` does not appear yet, click the refresh icon in the picker, or restart Positron - it scans for environments on startup
- You can now enter text and code into the notebook's entry fields, so called "cells". In order to test basic functionality, copy the code below into the first cell and execute it (by clicking the small "Play" button to the left of the cell, or pressing `Shift` + `Enter`).
	```python
	import matplotlib.pyplot as plt
	import numpy as np
	x = np.arange(10)
	y = np.arange(10)**2
	plt.plot(x, y, '-o')
	plt.show()
	```
If everything worked correctly, you should see a quadratic curve. You are ready to move on to the data science intro.

