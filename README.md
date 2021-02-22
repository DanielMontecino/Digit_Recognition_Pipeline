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

## Increasing and Changing the dataset

In cases where the dataset must be enlarged, it is usually necessary to add more images and modify the annotations. If the annotations are in a single file, the file is enlarged to contain the new annotations. On the other hand, if each annotation is a unique file, more files must be added, keeping the previous ones. 

DVC keeps track of both, that is, changes in files and of more files. To exten the dataset, unzip the extension.zip file, add the images of /extension/images/ to /dataset/images and add at the end of the file /dataset/labels/default.txt the content of /extension/labels/default.txt.

Now, the dataset has been modified, so the changes should be added and commited to dvc and git:

```
dvc add dataset/
git add dataset.dvc
git commit dataset.dvc -m "Extend the dataset"
```
NOTE: Remember to upload the modified dataset by executing ```dvc push```, and also the changes in the repository (```git push```)

Para ver la cantidad de archivos en ./dataset/images/ se puede usar ```ls ./dataset/images/ | wc -l```, mientras que para ver el tamaño del archivo con las etiquetas, se puede ejecutar: ```du -sh ./dataset/labels/default.txt```.

Sometimes it is necessary to change the existing dataset, instead of inlarging it. For example, if a label is wrong and it should be fixed. This is the case with the extension done before. Note that in the new ./dataset/labels.default.txt file, in lines 124, 125, and 126 the images are labeled as 1, when they are actually 4. 

Let's change these labels manually, changing the label 1 for 4 on the lines 124, 125 and 126. As before, any change in he dataset should be added to dvc and git:

```
dvc add dataset/
git add dataset.dvc
git commit dataset.dvc -m "Fix the extended dataset"
```
Now, are the changes are saved and commited

## Switching between workspace versions

What if we wanted to go back to a previous version of the dataset, for example, to reproduce previous results? Suppose we want to go back to the first version of the dataset, which was committed by "Add raw data".

to see previous version on git, you can type 
```
git log --oneline
```
This will show all the commits and their hashes, for example:
```
3f5dab5 (HEAD -> main, origin/main, origin/HEAD) Fix the extended dataset
5d90145 Extend the dataset
ea4eeda Configure remote storage
90487f0 Add raw data
1938909 Initial commit
```
So to go back to the original data, we should copy its hash (90487f0 on this example) and checkout the .dvc file in git:
```
git checkout 90487f0 dataset.dvc
```
And then, checkout with DVC:
```
dvc checkout
```
So, if you type ls ```./dataset/images/ | wc -l``` you can note that the size of the dataset is 100 again, and the size of the /dataset/labels/default.txt file is smaller.

Now, if we want to go to the first extended version (which has wrong labels), we can do the same, but with the respective hash.
```
git checkout 90487f0 dataset.dvc
dvc checkout
```
Then, we can see that /dataset/labels/default.txt file has the wrong labels in lines 124, 125, and 126 as before.
