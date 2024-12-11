# Mlops Template Project



## Getting started

- créez votre répertoir sur le git lab de l'université. 
- ajoutez une licence, un readme, un .gitignore pour python

## Squelette du projet

Afin de créer le squellete structuré et de manière automatique, dans votre projet clone sur votre machine ajouter un script ``template.py``
``` python

import os
from pathlib import Path
import logging

logging.basicConfig(level=logging.INFO, format='[%(asctime)s]:%(message)s:')

projet_name="gridPower" ## project name 

list_of_files =[
    ".github/workflows/.gitkeep",
    f"src/{projet_name}/__init__.py",
    f"src/{projet_name}/components/__init__.py",
    f"src/{projet_name}/utils/__init__.py",
    f"src/{projet_name}/utils/common.py",
    f"src/{projet_name}/config/__init__.py",
    f"src/{projet_name}/config/configuration.py",
    f"src/{projet_name}/pipeline/__init__.py",
    f"src/{projet_name}/entity/__init__.py",
    f"src/{projet_name}/entity/config_entity.py",
    f"src/{projet_name}/constants/__init__.py",
    "config/config.yaml",
    "params.yaml",
    "schema.yaml",
    "main.py",
    "app.py",
    "Dockerfile",
    "requirements.txt",
    "setup.py",
    "research/trails.ipynb",
    "tempaltes/index.html",
    "test.py",


]

for filepath in list_of_files:
    filepath=Path(filepath)
    filedir,filename= os.path.split(filepath)
    
    if filedir !="":
        os.makedirs(filedir,exist_ok=True)
        logging.info(f"Creating directory; {filedir} for the file {filename} ")
    

    if (not os.path.exists(filepath)) or (os.path.getsize(filepath)==0):
        with open(filepath,"w") as f:
            pass
            logging.info(f"Creating empty file :  {filepath}")
    else:
        logging.info(f"{filename} is already exists")

```


## Setup installation


Actualiser le fichier **requirements.txt** avec cette liste de libririe :

```
pandas 
mlflow==2.2.2
notebook
numpy
scikit-learn
matplotlib
python-box==6.0.2
pyYAML
tqdm
ensure==1.0.2
joblib
types-PyYAML
Flask
Flask-Cors
-e .
```
Puis, il faudra mettre à jour le fichier **setup.py** :

```python
import setuptools

with open('README.md','r',encoding="utf-8") as f:
    long_description=f.read()




__version__= "0.0.0"

REPO_NAME=''
AUTHOR_USER_NAME="asini"
SRC_REPO = "gridPower"
AUTHOR_EMAIL="aghilas.sini@univ-lemans.fr"

setuptools.setup(
    name=SRC_REPO,
    version=__version__,
    author=AUTHOR_USER_NAME,
    author_email=AUTHOR_EMAIL,
    description='a small  python package for  mlops ',
    long_description=long_description,
    long_destription_content='text/markdown',
    url="https://git.univ-lemans.fr/Aghilas.Sini/e2e-mlops",
    project_urls={

        "Bug Tracker":"https://git.univ-lemans.fr/Aghilas.Sini/e2e-mlops/-/issues",

    },
    package_dir={"":"src"},
    packages=setuptools.find_packages(where='src'),


)


```

En suit, excutez la commande suivante:


```bash
pip install -r requirements.txt
```



## Logging utilities

Dans le dossier src/{projet_name} mettre à jour le script '__init__.py' avec le contenu suivant :

```python

import os
import sys
import logging


logging_str = "[%(asctime)s:%(levelname)s: %(module)s: %(message)s]"

log_dir="logs"
log_filepath=os.path.join(log_dir,'running_logs.log')
os.makedirs(log_dir,exist_ok=True)


logging.basicConfig(

    level=logging.INFO,
    format=logging_str,

    handlers=[
        logging.FileHandler(log_filepath),
        logging.StreamHandler(sys.stdout)


    ]

)

logger= logging.getLogger('mlProjectLogger')

```

pour tester ce code, ouverez le script main.py:

```python

from gridPower import logger
logger.info('welcome to mlops courses')
```

Pour les fonctonnalités fréquement utilisées seront implémentées dans utils/common.py






