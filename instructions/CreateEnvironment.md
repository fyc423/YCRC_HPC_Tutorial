## Find Module
To list all available modules, run:
```
module avail
```
## Load Modules

Load the modules you need. For example, you can load a Conda module first:

```bash
module load miniconda   # or module load anaconda3
```

#### About the `miniconda` Module

The `miniconda` module provides a **minimal Conda setup** optimized for HPC environments.  
It includes:

- Conda (the package and environment manager)
- A lightweight Python interpreter
- Only core dependencies needed to run Conda

Most HPC workflows recommend **loading `miniconda`** and then creating custom environments for each project.

## Create a Conda Environment
Create a new Conda environment. Replace `myenv` with your desired environment name and specify any packages you need (e.g., `numpy`, `pandas`) or you can install the packages later. 
```
conda create --name myenv numpy pandas
```
To create an environment for your specific project. First navigate to your system project space and then create your project folder. 
To access system project path go to the *Files* tab in the portal and copy the path to your **system project space** which typically looks like:
```
/<filesystem>/<group>/project/<USERNAME>
```
Navigate to the system project sapce via 
```
cd /<filesystem>/<group>/project/<USERNAME>
```
Then make the project folder
```
mkdir -p ~/project
```
Then create a new Conda environment in the specified prefix. Replace `<project_name>` with your desired name for the project:
```
conda create --prefix ~/project/conda_envs/project_namev numpy pandas
```

#### Note: 
- project folder (`/<filesystem>/<group>/project/<USERNAME>`or similar):  
  Long-term, high-quota storage for code, datasets, Conda environments, Slurm scripts, logs, and results.  
  Accessible from all compute nodes.  
  Safe and persistent and files are *not* auto-deleted.

- scratch folder (`/<filesystem>/ysm/scratch/<USERNAME>` or similar):  
  High-speed temporary storage for large intermediate files, checkpoints, and short-term processing.  
  **Not** backed up; files may be auto-deleted after a period of inactivity.  
  Best used for temporary or high-throughput work.
  
## Create Jupyter Notebook
To use the Jupyter interface, you need to install Jupyter Notebook or Jupyter Lab.

```bash
module load miniconda
conda create --name myenv jupyter jupyter-lab
```

**OR**

You can first create an environment, then install Jupyter inside it:

```bash
module load miniconda
conda create --name myenv
conda activate myenv
conda install jupyter jupyter-lab
```

If you have installed or deleted a conda environment, run the following command in a terminal to update the Web Portal Access (OOD).
```
ycrc_conda_env.sh update
```
For creating an interactive Jupyter session using WebPortal please refer to [Interactive Web Portal Guildline](https://github.com/fyc423/YCRCClusterSetupTutorial/blob/main/instructions/InteractiveWebPortal.md)

## Install Monai
For `monai` user, after creating your `conda` environment, activate the environment then install `monai` in your specified environment:
```
conda activate myenv
conda install monai
```
Install any additional dependencies as needed. 

## Note for GPU-enabled packages installation

To install GPU-enabled packages such as `pytorch-gpu`, `tensorflow`, you may need to first [request a node in interactive mode with GPU](https://github.com/fyc423/YCRCClusterSetupTutorial/blob/main/instructions/InteractiveWebPortal.md) and then install it in the environment with GPU, otherwise, you may encounter an Error about missing CUDA kit.


Once you have started an interactive session **on a GPU node** and activated your Conda environment, follow the steps below to install GPU-enabled frameworks such as PyTorch and TensorFlow.


#### 1. Check the CUDA Version on the GPU Node

Run the following to confirm the CUDA version supported by the GPU driver:

```bash
nvidia-smi
```

Look for:

```
CUDA Version: 12.1
```

Use this CUDA version when choosing which PyTorch or TensorFlow build to install.


#### 2. Install GPU-Enabled PyTorch

Use the minimal official installation command, updating the CUDA version (`12.1`) to match your system:

```bash
conda install pytorch pytorch-cuda=12.1 -c pytorch -c nvidia
```

#### 3. Verify PyTorch GPU Support

```bash
python -c "import torch; print(torch.cuda.is_available())"
```

Expected output:

```
True
```


#### 4. Install GPU-Enabled TensorFlow

TensorFlow's GPU-enabled build is provided via pip:

```bash
pip install tensorflow[and-cuda]
```

If your environment requires older compatibility:

```bash
pip install tensorflow-gpu
```



