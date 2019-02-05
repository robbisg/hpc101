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
