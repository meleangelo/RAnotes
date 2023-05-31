---
output: 
  html_document: 
    highlight: zenburn
    theme: readable
---
# How to open an RStudio server on Rockfish

This brief note provides a quick tutorial on how to open an RStudio server instance in Rockfish and then provides some instructions.

The simplest way to do this is to run

```{linux}
r-studio-server.sh
```



This will generate a default RStudio server and produce the following output with few instructions.

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

This generates a RStudio server using 1 node, 1 core for 2 hours in the `defq` queue. This is the default request. One can change these parameters (see below).


To run the script, use the following:
```{linux}
sbatch R-Studio-Server.slurm.script
```

which will assign a number to the job
```{linux}
Submitted batch job 16396691
```

Now you look into the file generated
```{linux}
cat rstudio-server.job.16396691.out
```

which shows the following 

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


Now, open a new terminal in your computer. And run the first command

```{linux}
ssh -N -L 32865:c545:32865 amele1@login.rockfish.jhu.edu
```
you will need to login with your pwd for Rockfish.

To login into RStudio server, you just open a web browser and paste the address ```http://localhost:32865``` into the browser. Then you will have to use your Rockfish user and pwd to login. 


When you are done, please kill the server using the following command

```{linux}
scancel -f 16396691
```

If you do not kill the process, the server will run until the 2 hours expire (or more if you requested more than 2 hours..)

It is always a good idea to check the jobs running, to see if the server is open
```{linux}
sqme
```
