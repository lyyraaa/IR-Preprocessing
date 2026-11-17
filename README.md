# IR-Preprocessing

Codebase used to generate the results discussed in "Preprocessing and Model Generalisation in Deep Learning for Cancer Infrared Micro-Spectroscopy" in TODO, doi TODO.
The scripts contained in this repository will preprocess two QCL-IR hyperspectral image datasets, and then train and evaluated an MLP or U-Net model on the preprocessed data.

## Directory Structure

`src` contains utilities necessary to run experiments: preprocessing functions, PyTorch models, data loaders, etc.
`python_scripts` contains experiment code files.
`batch_scripts` contains the job scripts used to run the experiments on the University of Manchester HPC.

## Datasets

Data processed and preprocessed by these scripts is available at the following locations:
- NOCL/SALF/Salford TMA: 10.5281/zenodo.23915016 TODO
- AUS/Australian TMA: 10.5281/zenodo.23916491 TODO

## Running Experiments

Individual tests using one preprocessing pipeline were conducted by running one of batch scripts in the `batch_scripts` directory:
- `cpuscript_mlp` was used to run MLP tests on a CPU multicore node
- `gpuscript_unet` was used to run U-Net tests on a multicore node equipped with a GPU

Both of these scripts follow the same steps in sequence:
- create dataset directory structure on remote compute node
- copy metadata from raw dataset to remote compute node (annotations, etc.)
- copy experiment code files to remote compute node
- run preprocessing loop script on remote compute node
  - this loads an instance of raw data, preprocesses it, and then saves it on the remote node
- run classification script on remote compute node
- copy results files to local storage

Preprocessing scripts use command line arguments to control preprocessing parameters as so:
```bash
eval "$command $pipeline_use" sg waxremove rubberband 1st vectornorm
```
Likewise, classification scripts use command line arguments to control the random seed and channel-wise standardisation as so:
```bash
eval "$command" 0 "$standardise"
```

These scripts were written according to the specific directory structure of the UoM CSF. Getting these scripts to run elsewhere will require changing the following paths:
- `/mnt/eps01-rds/GardnerLab/qcl2026_raw` -> path to SALF dataset top-level directory
- `/mnt/eps01-rds/GardnerLab/aus_qcl2026_raw` -> path to AUS dataset top-level directory
- `/mnt/iusers01/eee01/w37262do/scratch/IR_preprocessing` -> path to this repository
- `/mnt/iusers01/eee01/w37262do/scratch/IR_preprocessing/PyIR/` -> path to PyIR spectral preprocessing library: https://github.com/dougalferg/PyIR
