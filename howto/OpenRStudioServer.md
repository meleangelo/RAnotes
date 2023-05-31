---
output: 
  html_document: 
    highlight: espresso
    theme: spacelab
---
# How to open an RStudio server on Rockfish

This brief note provides a quick tutorial on how to open an RStudio server instance in Rockfish and then provides some instructions.

### STEP 1: Run the RStudio server script

To open a session, run the following RStudio server script

```{linux}
r-studio-server.sh
```

After runnning the script  you will see the following output with few instructions:

```{linux}
Creating slurm script: R-Studio-Server.slurm.script


 The Advanced Research Computing at Hopkins (ARCH)
 SLURM job script for run RStudio into Singularity container
 Support:  help@rockfish.jhu.edu

 Nodes:       	1
 Cores/task:  	1
 Total cores: 	1
 Walltime:    	00-02:00
 Queue:       	defq

 The R-Studio-Server is ready to run.

 1 - Usage:

	 $ sbatch R-Studio-Server.slurm.script

 2 - How to login see login file (after step 1):

	 $ cat rstudio-server.job.<SLURM_JOB_ID>.out

 3 - More information about the job (after step 1):

	 $ scontrol show jobid <SLURM_JOB_ID>
```

This generates a RStudio server SLURM script using 1 node, 1 core for 2 hours in the `defq` queue. This is the default request. One can change these parameters (see below).

### STEP 2: Submit the RStudio Server job

To submit the RStudio server job, run the script using the following:
```{linux}
sbatch R-Studio-Server.slurm.script
```

which will assign a number to the job
```{linux}
Submitted batch job 16396691
```
In this case the job running the RStudio server is `16396691`.

### STEP 3: Check the info in the output file
Now you look into the file generated, that contains the instructions on how to access the RStudio server, 
```{linux}
cat rstudio-server.job.16396691.out
```

This file contains the following text and instructions:

```{linux}
Resetting modules to system default. Reseting $MODULEPATH back to system default. All extra directories will be removed from $MODULEPATH.

 The Advanced Research Computing at Hopkins (ARCH)
 SLURM job script for run RStudio into Singularity container
 Support:  help@rockfish.jhu.edu


1. SSH tunnel from your workstation using the following command:

   ssh -N -L 32865:c545:32865 amele1@login.rockfish.jhu.edu

2. log in to RStudio Server in your web browser using the Rockfish cluster credentials (username and password) at:

   http://localhost:32865

   user: amele1
   password: < Rochkfish password >

3. When done using RStudio Server, terminate the job by:

   a. Exit the RStudio Session ("power" button in the top right corner of the RStudio window)
   b. Issue the following command on the login node:

  scancel -f 16396691
```


### STEP 4: Access the RStudio Sever

1. Open a new terminal in your computer and run the command

```{linux}
ssh -N -L 32865:c545:32865 amele1@login.rockfish.jhu.edu
```
you will need to login with your pwd for Rockfish.

2. Open a web browser and past the address
```
http://localhost:32865
```
To login into RStudio server, use your Rockfish user and password.



### STEP 5: Terminate the RStudio Server
When you are finished using the RSTUDIO server, follow these steps

1. Exit the RStudio session, by clicking the power button on the top right corner of the RStudio window.

2. Run the folllowing command. 
```{linux}
scancel -f 16396691
```

- If you do not terminate the server, it will run until the 2 hours expire (or more if you requested more than 2 hours..)

- It is always a good idea to check the jobs running, to see if the server is open
```{linux}
sqme
```


## Additional customizations

When running the initial script, one can request different options. We can just check the help function 
```
r-studio-server.sh -h
```
and it will provide the options. 

```
usage: r-studio-server.sh [options]
                  [-n nodes] [-c cpus] [-m memory] [-t walltime] [-p partition] [-a account] [-q qos] [-g gpu] [-e email]

  Starts a SLURM job script to run R-Studio server into singularity container.


  options:
  ?,-h help      give this help list
    -n nodes     how many nodes you need  (default: 1)
    -c cpus      number of cpus per task (default: 1)
    -m memory    memory in K|M|G|T        (default: 4G)
                 (if m > max-per-cpu * cpus, more cpus are requested)
                 note: that if you ask for more than one CPU has, your account gets
                 charged for the other (idle) CPUs as well
    -t walltime  as dd-hh:mm (default: 00-02:00) 2 hours
    -p partition partition in defq|bigmem|a100 (default: defq)
    -a account   if users needs to use a different account and GPU.
                 Default is primary PI combined with '_' for instance:
                 <PI-userid>_gpu (default: none)
    -q qos       quality of Service's that jobs are able to run in your association (default: qos_gpu)
    -g gpu       specify GRES for GPU-based resources (eg: -g 1 )
    -e email     notify if finish or fail (default: <userid>@jhu.edu)
```

For example, the following command

```
r-studio-server.sh -n 1 -c 2 -m 8G -t 1-02:0 -p defq 
```

opens a session with 1 node, 2 cpus, 8GB of memory for 1 day and 2 hours, in the partition `defq`. 
