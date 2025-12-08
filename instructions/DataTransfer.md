# Data Transfer to the HPC

There are three primary ways to transfer data into the HPC environment. Choose the method that best fits your workflow and data size.

## Upload Files Directly Through the Web Interface (Open OnDemand)

The simplest method for small to medium files.

1. Log in to the HPC Open OnDemand portal.
2. Navigate to the **Files** tab.
3. Browse to the directory where you want to place your files.
4. Click **Upload** and select files from your local machine.

This method works best for small datasets (typically a few GB or less).

![Uploadl](https://github.com/fyc423/YCRCClusterSetupTutorial/blob/main/assests/Upload.JPG?raw=true)


## Transfer Files via Command Line 


#### Upload a file from your local machine to HPC:
```bash
scp /path/to/local/file username@cluster.address:/path/to/target/directory/
```

#### Upload a folder (recursive):
```bash
scp -r /path/to/local/folder username@cluster.address:/path/to/target/directory/
```

#### Example (from local machine):
```bash
scp mydata.csv netid@bouchet.ycrc.yale.edu:/home/netid/project/
```

This is often the most convenient method for command-line users.


## Transfer Large Data Using Globus

For transferring **large datasets**, `Globus` is the recommended tool.

`Globus` provides high-speed, fault-tolerant transfer between institutions, external storage, and the HPC environment.

Please see [Globus Documentation](https://docs.ycrc.yale.edu/data/globus/)  for more information.

