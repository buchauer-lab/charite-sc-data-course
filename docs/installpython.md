---
layout: page
permalink: /installpython/
---

#### Instructions for installing python and the single-cell data analysis kit scanpy to your laptop (local installation)

#### Step 1 - install miniforge3

We will first install `miniforge` which is itself an installer for `mamba` which is an installer for python packages (kind of like a kinase kinase kinase).
- Go to [https://github.com/conda-forge/miniforge](https://github.com/conda-forge/miniforge) and scroll down to see the install instructions for your system
- For Windows, install from the executable installer with default settings
- For macOS, you need to open the application "Terminal", e.g. via the Launchpad, and enter the command starting with `curl` followed by the command starting with `bash`. Please ask an instructor for help if you are using this workflow.

To verify miniforge is installed correctly:
- Open a Terminal:
	- If on Windows, open the Software "Miniforge Prompt" which we have just installed
	- If on MacOS / Linux, open a regular terminal
- Type `mamba --version` and press Enter
- You should see a version number displayed


#### Step 2 - create an environment for your data analysis

The package installer `mamba` allows you to create separate analysis environments into which you can install required software for data analysis. The advantage of this approach is that different projects, which might require different versions of the same software package, are kept separate and cannot interfere negatively with one another.
- Open a terminal (as described in Step 1)
- type each of the following lines and hit enter.
	- `mamba create -n scrnaseq python=3.10`(this creates an analysis environment called `scrnaseq`)
    - when prompted, type 'y' and press Enter to proceed
	- `mamba activate scrnaseq`(this activates the environment, which should become evident by the environment name now preceding the prompt)
- Install core packages required for our course
 ```bash
 mamba install -c conda-forge -c bioconda \ 
	 scanpy \ 
	 pandas \ 
	 numpy \ 
	 matplotlib \ 
	 seaborn \ 
	 jupyter \ 
	 notebook \ 
	 scikit-learn \ 
	 scipy \ 
	 h5py \ 
	 openpyxl \ 
	 leidenalg \ 
	 python-igraph \ 
	 -y
```

#### Step 3 - open a jupyter notebook and start coding

We will use jupyter notebooks as our code editor for this course. The notebook is served in a web browser after a call from the terminal.
- Use the same terminal as above (in which your analysis environment has already been activated), or, if using a newly started terminal instance, activate your environment in it by typing and executing
	- `mamba activate scrnaseq`
- Within the `scrnaseq` environment, type the following command and hit enter
	- `jupyter notebook`
- A link containing `localhost` will be displayed - copy this link into the address line of a web browser of your choice and hit enter
- Your notebook will open and you can enter text and code into its entry fields, so called "cells". In order to test basic functionality, copy the code below into the frist cell and execute it (by clicking the small "Play" button in the line just above the cell).
	```python
	import matplotlib.pyplot as plt
	import numpy as np
	x = np.arange(10)
	y = np.arange(10)**2
	plt.plot(x, y, '-o')
	plt.show()
	```
If everything worked correctly, you should see a quadratic curve. You are ready to move on to the data science intro.

