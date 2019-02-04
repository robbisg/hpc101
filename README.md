# hpc101
This is a how-to guide to install stuff for HPC on virtual boxes mounting Ubuntu 18.04

## Preliminary operations
Install Ubuntu 18.04 on a single virtual machine, I enabled two network interfaces on Virtual box on both nodes, but on master node I used a bridged connection so that the VM is in the network of the host computer and thus accessible via ssh, while a NAT connection can be used for the node(s). The second interface is for the internal network.

The node machine can be cloned starting from the master, be careful to use a different MAC address when cloning the master.

## After cloning operations

