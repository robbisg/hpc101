# Slim guide to Anaconda environments
This small guide is used to build a anaconda environment in python. I used to build a file named ```neuropy.yml```.
For example a basic environment file could be:

```yml
name: neuropy
dependencies:
  - python=3.6
  - pip
  - numpy
  - scipy=1.2
  - matplotlib
  - cython
  - tk
  - pip:
     - ipython
     - mne
     - pymvpa2
     - seaborn
     - nibabel
     - nitime
     - imbalanced-learn
     - pandas
     - bokeh
     - dask
     - dask-jobqueue
     - dask-kubernetes
     - distributed
     - joblib
     - "--editable=git+https://github.com/robbisg/scikit-learn.git@group_cv#egg=scikit-learn"
     - "--editable=git+https://github.com/robbisg/nilearn.git@scores_sl#egg=nilearn"
```
You can add and remove package at will!
Packages with ```--editable``` are downloaded from ```github``` and installed, the symbol ```@``` is used to select
a particular branch if you need it, while ```egg``` keyword is used to give a namespace to this package.

Then you can create the environment using:
```bash
conda env create -f neuropy.yml
```
To enter the environment
```bash
conda activate neuropy
```
This is used to exit the environment
```bash
conda deactivate
```
and to update the environment:
```bash
conda env update -f=neuropy.yml
```

If you need to list all the environments
```bash
conda env list
```

While if you need to remove the environment just do:
```bash
conda remove --name myenv --all
```
# Triton commands


```
module load miniconda 
conda env create -n test_env --file requirements.yml
```
https://scicomp.aalto.fi/triton/tut/storage.html#rsync

```
module load miniconda 
conda env create -n test_env --file requirements.yml
source activate test_env
```
https://scicomp.aalto.fi/triton/apps/jupyter.html#installing-kernels-from-virtualenvs-or-anaconda-environments
https://scicomp.aalto.fi/triton/tut/storage.html#remote-mounting-using-smb
```
envkernel conda --user --name test_env --display-name "test_env" ~/.conda/envs/test_env
conda install jupyter
```
