# SLURM Configuration of Virtual box cluster

## Setup
Install munge that is used by slurm as authentication service, on all nodes. If you are using a Virtual box cluster, install it on one virtual machine and then clone it.
```bash
sudo apt-get install munge slurm-wlm
```

