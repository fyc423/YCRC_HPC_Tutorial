Once your code is ready to run you can submit your script as a batch job. 
## Submission Script
Write a .sh script that specifies the resources required and commands to run. 
```
#!/bin/bash
#SBATCH --job-name=job_name             # Job name
#SBATCH --mail-type=FAIL,END            # Email if job fails or ends  
#SBATCH --mail-user=first.last@yale.edu # Your email address

#SBATCH --partition=gpu                 # Partition (queue) name, usually gpu
#SBATCH --nodes=1                       # Number of nodes
#SBATCH --ntasks=1                      # Number of tasks (usually 1 for GPU jobs)
#SBATCH --cpus-per-task=4               # Number of CPU cores per task
#SBATCH --mem-per-cpu=1G                # Memory per CPU core
#SBATCH --time=2-00:00:00               # Total run time (HH:MM:SS) For GPU, maximum time will be 2-00:00:00   
#SBATCH --gres=gpu:1                    # Number of GPU (request more than one only for distributed parallel training/computing)

#SBATCH --output=logs/%x_%j.out         # Standard output file (%x for job name, %j for job ID)
#SBATCH --error=logs/%x_%j.err          # Standard error file

# Navigate to the directory containing my_script.py
cd /my_path/

# Load necessary modules or software
module load miniconda 

# Activate your Conda environment 
conda activate my_env

# Run your program or script
python my_script.py --arg1 value1 --arg2 value2
```
You can upload the submission script directly to the desired folder via the web portal or using `scp` or `rsync`.

Navigate to the location of .sh file and run the .sh script via

```
sbatch my_slurm.sh
```

## Check Job Progress
To check your job status, you can access `Active Jobs` on the web portal. 
Or create an alias that shows job queue information for the user identified by NetID. 
```
alias sq='squeue -u NetID --format="%.10i %.9P %.25j %.65k %.5u %.8T %.3C %.10M %.9l %.6D %R“’
```
And replace NetID with your own netid.

- `%.10i` - Job ID
- `%.9P` - Partition name, limited to 9 characters
- `%.25j` - Job name, limited to 25 characters
- `%.65k` - Requested features (node types or resources), limited to 65 characters
- `%.5u` - Username, limited to 5 characters
- `%.8T` - Job state, limited to 8 characters (e.g., RUNNING, PENDING)
- `%.3C` - Number of CPUs allocated to the job, limited to 3 characters
- `%.10M` - Time used by the job, limited to 10 characters
- `%.9l` - Time limit, limited to 9 characters
- `%.6D` - Number of nodes allocated to the job, limited to 6 characters
- `%R` - Reason or assigned node (for pending jobs)

To view output and error files after the job completes:
```
cat logs/%x_%j.out    # View output
cat logs/%x_%j.err   # View error
```
Or follow in real-time if the job is still running:
```
tail -f logs/%x_%j.out    # Follow output
tail -f logs/%x_%j.err   # Follow error
```
Or you can access the output and error files via the web portal or interactive apps such as Jupyter and Code Server.

You may also check the job resource usage with the following command:
```
jobstats JobID
```
where the JobID is the id of the corresponding job. This command will return you the CPU, memory, GPU, and GPU memory usage for the job and recommend settings to optimize your usage.

## Monitor GPU Availability

you may use the following script to monitor the GPU availability on the cluster:
```
sinfo -e -o "%.6D|%T|%C|%G|%b" | column -ts "|" | grep -v ',pi' | grep GPU_ID
```
You can then replace GPU_ID with the name of the GPU that you want to use, e.g., a5000/rtx5000. This will return you with the availability of the indicated GPU.

- `-o "%.6D|%T|%C|%G|%b"` - formats output to include:
  - Node Count (`%D`)
  - Node State (`%T`)
  - CPU Usage (`%C`: allocated/idle/other/total)
  - GPU Resources (`%G`: type and count, e.g., `gpu:h200:8`)
  - Node Features (`%b`, such as CPU generation or GPU model)
- `column -ts "|" ` - aligns columns for readability.
- `grep -v ',pi'` - removes nodes tagged with internal or special-purpose designations.
- `grep GPU_ID` - filters output to only list GPU nodes.

