# ManyDG
- ManyDG: Many-domain Generalization for Healthcare Applications
- Data, Processing Scripts, Baselines and Model Codes
> 4-minute explanation video is provided in [YouTube]
https://youtu.be/rbTBajnZV5U

## 1. Folder Tree
- ```data/```
    - ```drugrec/```
        - this is the folder stores data for drug recommendation task
        - **data_process.py**: this file is used for processing MIMIC-III data for the task
    - ```Seizure/```
    - ```sleep/```
        - this is the data folder for the processed sample-based Sleep-EDF cassette portion.
        - **sleep_edf_process.py**: the processing file for sleep-edf (using multiprocess package for parallel programming)
        - for downloading the Sleep-EDF data, please refere to https://www.physionet.org/content/sleep-edfx/1.0.0/
    - ```hospitalization/```
        - this is the folder for the processed eICU data
        - **eICU_process_step1.py**: the first processing step of eICU data
        - **eICU_process_step2.py**: the second processing step of eICU data
    - ```idxFile/```
        - this folder stores the code mapping of different event types in eICU dataset. They will be generated from **eICU_process_step1.py** and **eICU_process_step2.py**. They are used to assign feature dimensions for hospitalization prediction task
- ```log/```, ```pre-trained/```
    - this two folders stores the automatically generated running logs and pre-trained models
- **model.py**
    - this file contains all the backbone and Base models for running the experiments
- ```run_drugrec/```
    - **model_drugrec.py**: This file inherit the Base model and other model utility functions from **model.py** and support the drug recommendaton task
    - **run_drugrec.py**: This is the entry of drug recommendation task, specifiying the data loader and other initialization steps.
    - **utils_drugrec.py**: This file provides the data split and loading files.
- ```run_hospitalization/```
    - **model_hospitalization.py**: This file inherit the Base model and other model utility functions from **model.py** and support the hospitalization prediction task
    - **run_hospitalization.py**: This is the entry of hospitalization prediction task, specifiying the data loader and other initialization steps.
    - **utils_hospitalization.py**: This file provides the data split and loading files.
- ```run_seizure/```
    - **model_seizure.py**: This file inherit the Base model and other model utility functions from **model.py** and support the seizure detection task
    - **run_seizure.py**: This is the entry of seizure detection task, specifiying the data loader and other initialization steps.
    - **utils_seizure.py**: This file provides the data split and loading files.
- ```run_sleep/```
    - **model_sleep.py**: This file inherit the Base model and other model utility functions from **model.py** and support the sleep staging task
    - **run_sleep.py**: This is the entry of sleep stagin task, specifiying the data loader and other initialization steps.
    - **utils_sleep.py**: This file provides the data split and loading files.

## 2. How to run the code
 
- For sleep staging
``` python
cd ./ManyDG
python data/sleep/sleep_edf_process.py
python run_sleep/run_sleep.py --model [MODEL] --cuda [WHICH GPU] --N_pat [N_OF_PAT] --epochs [EPOCHS]

usage: sleep_edf_process.py [-h] --model MODEL [--cuda CUDA] [--N_pat N_PAT] [--dataset DATASET] [--MLDG_threshold MLDG_THRESHOLD]
               [--epochs EPOCHS]

optional arguments:
  -h, --help            show this help message and exit
  --model MODEL         choose from base, dev, condadv, DANN, IRM, SagNet, PCL, MLDG
  --cuda CUDA           which cuda
  --N_pat N_PAT         number of patients
  --dataset DATASET     dataset name
  --MLDG_threshold MLDG_THRESHOLD
                        threshold for MLDG
  --epochs EPOCHS       N of epochs
```


- for seizure detection
``` python
cd ./ManyDG
# obtain the Seizure data first
python run_seizure/run_seizure.py --model [MODEL] --cuda [WHICH GPU] --N_vote [DEFAULT 5] --N_pat [N_OF_PAT] --epochs [EPOCHS]

```

- for drug recommendation
``` python
cd ./ManyDG
python data/drugrec/data_processing.py
python run_drugrec/run_drugrec.py --model [MODEL] --cuda [WHICH GPU] --N_pat [N_OF_PAT] --epochs [EPOCHS]
```
- for hospitalization prediction
``` python
cd ./ManyDG
python eICU_process_step1.py
python eICU_process_step2.py
python run_hospitalization/run_hospitalization.py --model [MODEL] --cuda [WHICH GPU] --N_pat [N_OF_PAT] --epochs [EPOCHS]
```
