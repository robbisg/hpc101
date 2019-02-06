# Open MPI + NFS server

I also checked if Open MPI can be used with slurm to submit jobs.

After several errors I ended up with a clear install pipeline!

## Setup NFS

Open MPI programs needs to be shared across all nodes, so the best solution is to share a directory through NFS utility.

### Master node
```bash
sudo apt-get install nfs-kernel-server
mkdir /home/robbis/shared
sudo cat /etc/exports
# /etc/exports: the access control list for filesystems which may be exported
#		to NFS clients.  See exports(5).
/home/robbis/shared 192.168.100.0/24(rw,sync,no_root_squash,no_subtree_check)
sudo exportfs -a
sudo systemctl restart nfs-server.service
```

In the lines above, I installed the nfs-server, created the directory, looked at export file to check if directory is currently shared (you need to modify that file), applied changes and restarted the server.

### Slave nodes
```bash
sudo apt-get install nfs-common
mkdir /home/robbis/shared
sudo mount master:/home/robbis/shared/ /home/robbis/shared/
```
Instead of ```master``` you can put the ip of the master node or use some aliases that must be included in ```/etc/hosts``` file.

## Setup Open MPI

I think that Open MPI is installed with Kubernetes or Slurm, I don't know, but if they didn't, run on all nodes:
```bash
sudo apt-get install libopenmpi-dev
```

### Check Open MPI with Slurm
I found some examples [here](https://computing.llnl.gov/tutorials/mpi/exercise.html) and run a simple program on my virtual cluster.
```bash
cd shared/
```

Create a ```mpi_hello.c```
```c
/******************************************************************************
* FILE: mpi_hello.c
* DESCRIPTION:
*   MPI tutorial example code: Simple hello world program
* AUTHOR: Blaise Barney
* LAST REVISED: 03/05/10
******************************************************************************/
#include "mpi.h"
#include <stdio.h>
#include <stdlib.h>
#define  MASTER         0

int main (int argc, char *argv[])
{
int   numtasks, taskid, len;
char hostname[MPI_MAX_PROCESSOR_NAME];

MPI_Init(&argc, &argv);
MPI_Comm_size(MPI_COMM_WORLD, &numtasks);
MPI_Comm_rank(MPI_COMM_WORLD,&taskid);
MPI_Get_processor_name(hostname, &len);
printf ("Hello from task %d on %s!\n", taskid, hostname);
if (taskid == MASTER)
   printf("MASTER: Number of MPI tasks is: %d\n",numtasks);
MPI_Finalize();

}
```

Then a slurm batch file ```submit.sh```:
```bash
#!/bin/bash
##############################################################################
# FILE: batchscript.c
# DESCRIPTION:
#   Simple Slurm job command script for MPI exercise
# AUTHOR: Blaise Barney
# LAST REVISED: 05/23/17
##############################################################################

##### These lines are for Slurm
#SBATCH -N 2                      #Number of nodes to use
#SBATCH -p debug              #Workshop queue
#SBATCH -t 5:00                   #Maximum time required
#SBATCH -o output.%j              #Output file name

### Job commands start here 
### Display some diagnostic information
echo '=====================JOB DIAGNOTICS========================'
date
echo -n 'This machine is ';hostname
echo -n 'My jobid is '; echo $SLURM_JOBID
echo 'My path is:' 
echo $PATH
echo 'My job info:'
squeue -j $SLURM_JOBID
echo 'Machine info'
sinfo -s

echo '=====================JOB STARTING=========================='

### CHANGE THE LINES BELOW TO SELECT DIFFERENT MPI CODES AND/OR COMPILERS
# cd ~/mpi
#Compile an exercise code
mpicc -O3 -w -g mpi_hello.c -o mpi_hello
#Run the code
srun -N2 -n2 ./mpi_hello

### Issue the sleep so we have time to see the job actually running
sleep 120
echo ' ' 
echo '========================ALL DONE==========================='
```

Be careful to change the ```#SBATCH -p debug``` line that indicates the partition of your cluster to use with this script.
Then run:

```
chmod +x submit.sh
sbatch submit.sh
# Submitted batch job 38
```

Then the output is in ```output.38```.
