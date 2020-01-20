# Triton AALTO stuff

```bash
$ ssh username@triton.aalto.fi
$ hostname
login2.triton.aalto.fi
```

```
$ module load anaconda3

Other versions
$ module load matlab/r2017b 
```

Quota Linux to manage storage disks!
smb: used for mounting

### SLURM
```bash
$ slurm history {1year|1month|etc}
Shows the story of lines sent to triton
$ sacct
Kind of slurm history
$ seff $PID
Efficiency and statistics for task PID
```
