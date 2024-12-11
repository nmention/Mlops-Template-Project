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
```python 

import os
from box.exceptions import BoxValueError
import yaml
from gridPower import logger
import json
import joblib
from ensure import ensure_annotations
from box import ConfigBox
from pathlib import Path
from typing import Any



@ensure_annotations
def read_yaml(path_to_yaml: Path) -> ConfigBox:
    """reads yaml file and returns

    Args:
        path_to_yaml (str): path like input

    Raises:
        ValueError: if yaml file is empty
        e: empty file

    Returns:
        ConfigBox: ConfigBox type
    """
    try:
        with open(path_to_yaml) as yaml_file:
            content = yaml.safe_load(yaml_file)
            logger.info(f"yaml file: {path_to_yaml} loaded successfully")
            return ConfigBox(content)
    except BoxValueError:
        raise ValueError("yaml file is empty")
    except Exception as e:
        raise e
    


@ensure_annotations
def create_directories(path_to_directories: list, verbose=True):
    """create list of directories

    Args:
        path_to_directories (list): list of path of directories
        ignore_log (bool, optional): ignore if multiple dirs is to be created. Defaults to False.
    """
    for path in path_to_directories:
        os.makedirs(path, exist_ok=True)
        if verbose:
            logger.info(f"created directory at: {path}")


@ensure_annotations
def save_json(path: Path, data: dict):
    """save json data

    Args:
        path (Path): path to json file
        data (dict): data to be saved in json file
    """
    with open(path, "w") as f:
        json.dump(data, f, indent=4)

    logger.info(f"json file saved at: {path}")




@ensure_annotations
def load_json(path: Path) -> ConfigBox:
    """load json files data

    Args:
        path (Path): path to json file

    Returns:
        ConfigBox: data as class attributes instead of dict
    """
    with open(path) as f:
        content = json.load(f)

    logger.info(f"json file loaded succesfully from: {path}")
    return ConfigBox(content)


@ensure_annotations
def save_bin(data: Any, path: Path):
    """save binary file

    Args:
        data (Any): data to be saved as binary
        path (Path): path to binary file
    """
    joblib.dump(value=data, filename=path)
    logger.info(f"binary file saved at: {path}")


@ensure_annotations
def load_bin(path: Path) -> Any:
    """load binary data

    Args:
        path (Path): path to binary file

    Returns:
        Any: object stored in the file
    """
    data = joblib.load(path)
    logger.info(f"binary file loaded from: {path}")
    return data



@ensure_annotations
def get_size(path: Path) -> str:
    """get size in KB

    Args:
        path (Path): path of the file

    Returns:
        str: size in KB
    """
    size_in_kb = round(os.path.getsize(path)/1024)
    return f"~ {size_in_kb} KB"

```

ce code permet de factoriser la lecture des fichiers de configuration de manière sure.


## Project WorkFlow

Ci-dessous la procedure à suivre pour mettre en place chaque étape de la chaine de traitement :

1. Update config.yaml
2. Update schema.yaml
3. Update params.yaml
4. Update the entity
5. Update the configuration manager in src config
6. Update the components
7. Update the pipeline 
8. Update the main.py
9. Update the app.py


### Research  pour  prototyping
 
1. l'étape d'ingestion de donnée 

Configuration 

```yaml 
artifacts_roots: artifacts

data_ingestion:
  root_dir : artifacts/data_ingestion
  source_URL: https://github.com/entbappy/Branching-tutorial/raw/master/winequality-data.zip
  local_data_file : artifacts/data_ingestion/data.zip
  unzip_dir : artifacts/data_ingestion 
```



 



