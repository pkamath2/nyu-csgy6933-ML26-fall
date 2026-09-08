# Fall 2026 - CSGY-6933: Machine Listening 
**Course Instructor:** Juan Bello

**Teaching Assistant:** Purnima Kamath 


This repository (all notebooks and instructions) was originally created by Julia Wilkins [@juliawilkins](https://github.com/juliawilkins) for the spring 2026 semester

----


Welcome to Machine Listening! In this repository you'll find the assignment Jupyter notebooks and any supplemental material you might need to get started on the course assignments. Note that the notebooks will only be visible here at the time of assignment release.

Steps to completing and turning in your assignments: 

1. Download the necessary directory from this repository (e.g. `assignment`/, contains `assignment1.ipynb` and any needed files). 
2. Run the Jupyter notebook for the assignment locally (guide below) and complete the code and written questions.
3. Make sure your full notebook is **evaluated**.
4. Submit the **evaluated** `.ipynb` file to Brightspace.
----

Below I've put together a guide for working with Jupyter notebooks:

## Jupyter Notebook Setup Guide

For the class assignments, we will be using Python in Jupyter notebooks. I recommend using Anaconda to create distinct virtual environments with only the packages specific to a specific project/set of projects installed, instead of installing packages to your root environment. This tutorial will step you through that workflow.

Disclaimer: there are *many* ways to get up and running with Jupyter Notebook and your Python package manager of choice. This is just *one* way if you need a guide. If you already have your own setup that works for you, stick with it! 🙂 

### Pre-requisites:

- Python ≥ 3.9. Verify with `python --version` on the command line.
- `pip`: Python package manager. Verify with `which pip` or `pip --version` to see your pip path and confirm installation.
- Check if you have anaconda installed already: `conda --version`
    - → If yes: check if you can already run jupyter notebook: `jupyter notebook`
        - → If yes, you can skip this whole tutorial! (unless you want tips on running notebooks using specific conda environment as jupyter kernels)
    - → If no: yay, you’re in the right place. Follow this tutorial!

### Installing Jupyter Notebook with Anaconda

1. Install Anaconda if you don’t already have it : [https://www.anaconda.com/download](https://www.anaconda.com/download) (tip: you can skip the download registration, you shouldn’t need to create an account). More tips [here.](https://docs.jupyter.org/en/latest/install/notebook-classic.html#installing-jupyter-using-anaconda-and-conda)
    1. At this point you should be able to run `jupyter notebook` from the CLI and open a notebook using a root Python kernel. You could optionally stop here, but I highly advise continuing with a few more steps so that you can create kernels using a custom conda environment with just the packages you need for this course.
2. In the CLI: create a new conda environment with Python 3.9: `conda create -n MLenv python=3.9`
3. Activate this environment: `conda activate MLenv`
4. Install ipykernel for conda: `conda install ipykernel`
5. Install ipykernel for conda: `conda install notebook`
6. Install nb_conda_kernels for conda: `conda install nb_conda_kernels`
7. Install some base packages for Python that will be useful for this class in this environment using `pip`** (i.e. with `conda activate MLenv` activated):
    
    ```bash
    pip install matplotlib
    pip install librosa
    pip install scipy
    pip install scikit-learn
    ```
    
8. Run `jupyter notebook` in your conda environment. This should automatically launch a jupyter notebook in your browser, hosted locally.
9. Under the `kernel` dropdown menu in the Jupyter UI, select `Change kernel`. You should see the option to select your new conda environment here, something like `Python [conda env:MLenv]` . Now you can use the packages installed in your conda environment. 
8. When you need to install more packages in this environment, you can go back to the CLI, activate your conda environment, and `pip` install them. You will have to restart your kernel after this to be able to import the new packages!

**✅🎉 Congrats! You should now have a working Jupyter notebook setup that you can use for class assignments.✅🎉**

** **Note** that installing Python packages with `pip` inside of a conda environment is not always advisable, as you can also install most packages with `conda install` and it can get messy if you are installing packages with both. Installing everything via `pip` is simply my preferred method and what has worked for me in the past.
