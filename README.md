# Digit_Recognition_Pipeline
Simple project for testing ML pipeline

# DVC

## Instalattion
For more details, visit the official webpage: https://dvc.org/doc/install

For Linux, DVC can be installed with pip, conda, snap, or download the package from the webpage. DVC can be installed in a virtual environment:
* pip: ```pip install dvc```
* conda: ```conda install -c conda-forge dvc```
* snap: ```snap install --classic dvc```

## Get started

To init DVC in a git project, inside the git project run:
```
dvc init
```
This will create some file in the project, so a commit should be done.

To add a dataset to DVC, type (In the case of this project, DATASET_PATH is just ./dataset.):

```
dvc add DATASET_PATH
```
This will add the files in DATASET_PATH to DVC and will modify the dvc files. As this dvc files are part of the git project, a new commit should be done (this commiit is suggested by DVC after adding the dataset).

```
git add .gitignore dataset.dvc
git commit -m "Add raw data"
```

Then, if the local repository is not the final storage of the dataset, the dataset can be uploaded and linked to the project by:

```
dvc remote add -d storage REMOTE_ADRESS
```
REMOTE_ADRESS is where the dataset will be stored. 

As this action will change a dvc file in the git project, this file should be added and commited in git:

```
git add .dvc/config
git commit -m "Configure remote storage"
```
To upload the dataset to the remote storage, just type:
```
dvc push
```

To retrieve the dataset from the remote storage by DVC, after doing a git clone, and git pull, do:

```
dvc pull
```
