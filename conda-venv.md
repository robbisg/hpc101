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

Then you can do:

```bash
conda env create -f neuropy.yml
conda activate neuropy
conda deactivate
conda env update -f=neuropy.yml
```

First guy create the environment, after creation you can enter by using second guy and leave using third. Last line
is used to update the environment.
