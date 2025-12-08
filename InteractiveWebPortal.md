## Interactive Sessions
You can use the [Web Portal (Open OnDemand)](https://docs.ycrc.yale.edu/clusters-at-yale/access/ood/), and create an interactive session for the specific cluster you are using. This can be used during the development phase of your code for testing and troubleshooting. 

You can also use the web portal to 
- Check `Files` 
- Check active jobs under `Jobs`
- Check your user portal summary under `Utilities`

![WebProtal](https://github.com/fyc423/YCRCClusterSetupTutorial/blob/main/ood_welcome.png?raw=true)

# Interactive Apps
The HPC Open OnDemand portal provides several interactive apps, popluarly used ones including:
- **Remote Desktop**
- **RStudio**
- **Jupyter / JupyterLab**
- **MATLAB**
- **Code Server (VS Code)**

All of these apps share **similar resource request settings**.  
Below is an explanation of each launch option using Remote Desktop as an example.

- `Number of hours`: Sets the maximum wall time for the interactive session. Once this time is reached, the job will end automatically. For `gpu_devel` partitions, the maximum number of hours is 6.
- `Number of CPU cores per node`: Requests the number of CPU cores available to your session. If your job can run on multiple nodes (in a distributed fashion), you could adjust this number accordingly.
- `Memory per CPU core (GiB)`: Specifies how much RAM is allocated per CPU core. Total memory = (number of cores × memory per core). Increase this for large datasets or memory-intensive tasks. This is helpful for jobs that require parallel processing or multi-threading.
- `Partition`: Selects which queue or node type to run on (e.g., `devel`, `general`, or `gpu`). Using GPUs requires selecting a GPU partition.For the development phase, select `gpu_devel`

### Optional
- `Reservation`: Used only when running jobs during a scheduled reservation (such as a class or workshop). Leave blank unless instructed otherwise.
- `Email notification`: Sends an email once the session begins running. Useful when the queue is busy.
- `Slurm account`: Specifies which allocation or research group is charged for compute usage. Leave blank unless you belong to multiple accounts. 
- `Additional job options`: Allows adding advanced Slurm flags (one per line), such as GPU constraints or hardware requirements.

![PortalSetupl](https://github.com/fyc423/YCRCClusterSetupTutorial/blob/main/PortalSetup.JPG?raw=true)

This is equivalent to submitting an interactive job on commend line:
```
salloc --partition=gpu_devel --time=6:00:00 --nodes=1 --cpus-per-task=8 --mem-per-cpu=1g
```
- `partition`: Partitions

- `time`:  Number of hours

- `nodes`: Number of nodes *(For most interactive sessions, this **must be 1**, because these applications must run entirely on a single node and cannot span multiple machines.)*

- `cpus-per-task`: Number of CPU cores per node

- `mem-per-cpu`: Memory per CPU core in GiB

## Partitions Briefly Explained

- `day`: Short CPU jobs (up to a day). Good for routine workloads.  Default if you don't specify one with --partition

- `devel`: Short, fast-queue CPU jobs for testing, debugging, or quick Jupyter sessions.

- `week`: Long CPU jobs that may run for several days.

- `gpu`: Standard GPU nodes for longer training or production GPU workloads.

- `gpu_h200`: High-performance next-generation GPU nodes (e.g., H100/H200). Use for large or advanced GPU workloads.

- `gpu_devel`: Short, fast-queue GPU jobs for testing, debugging, installing GPU frameworks, or quick Jupyter GPU sessions (typically ≤ 6 hours).

- `bigmem`: High-memory CPU nodes for large datasets or memory-intensive tasks.

- `mpi`: Multi-node distributed compute jobs using MPI. Not for Jupyter or interactive apps.

- `scavenge`: Low-priority CPU jobs using idle nodes; may be preempted.

- `scavenge_gpu`: Low-priority GPU jobs using idle GPU nodes; may be preempted.

