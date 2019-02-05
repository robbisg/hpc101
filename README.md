# hpc101
This is a how-to guide to install stuff for HPC on virtual boxes mounting Ubuntu 18.04

## Preliminary operations
Install Ubuntu 18.04 on a single virtual machine, I enabled two network interfaces on Virtual box on both nodes, but on master node I used a bridged connection so that the VM is in the network of the host computer and thus accessible via ssh, while a NAT connection can be used for the node(s). The second interface is for the internal network.

The node machine can be cloned starting from the master, be careful to use a different MAC address when cloning the master.

## After cloning the machine
The cloned machine now has the same hostname and the same network settings of the master machine, so we need to change the hostname by typing ```sudo hostnamectl set-hostname node01```.  
Then we need to enter ```sudo nano /etc/cloud/cloud.cfg``` and to add ```preserve_hostname: true``` (or changing it).

Finally, we need to modify network settings. 

Typing ```sudo nano /etc/netplan/50-cloud-init.yaml``` we are able to edit the configuration.

My configuration file looks like this. 
```yaml
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        enp0s3:
            addresses: []
            dhcp4: true
        enp0s08:
            addresses: [192.168.100.101/24]
            dchp4: no
    version: 2
 ```
Obvioulsy you can change the addresses of your nodes as you like.  
Then type ```sudo netplan apply``` to apply your edits (type ```ifconfig``` to validate them).

Editing the file ```/etc/hosts``` is not mandatory, it can be useful to let the nodes be accessible by their name rather than their IPs.

Now you can setup [Kubernetes](kubernetes.md), [Slurm](slurm.md).

