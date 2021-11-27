# Vagrant setup to create a local kubernetes lab

This repo aims to quickly setup a kubernetes lab environment mainly for learning purposes.
The goal is to create a K8s cluster with this configuration:
1 control panel node
3 worker nodes


## Requirements

### Install
The host must have :
* Vagrant to handle virtual machines
* VirtualBox as Hypervisor
* Ansible for provisionning

### Host system requirements
OS : Linux *I use pop OS 20.04*
CPUs number : 12 (at least)
RAM : 16GB (at least)


*Notes*:
Activate

```sh
export VAGRANT_EXPERIMENTAL="disks"
```

I use virtualbox as virtual machines hypervisor.
As I have disk space limitations on my desktop, I did the following setup via CLI to use my external drive as a disk space extension:

```sh
# Check the current default path for the VMs
vboxmanage list systemproperties | grep 'Default machine folder'

# Set a new path for the VMs to be created.
# Check the external drive(s) mount path with `df -h` or `fdik -l`
vboxmanage setproperty machinefolder <path/to/the/external_drive>

# Ensure that the new setting is applied
vboxmanage list systemproperties | grep 'Default machine folder'
```

## Creating the cluster

```sh
# Vagrant up!
vagrant up

# To connect with one of the nodes, use the names c1-cp1, worker-1, worker-2, worker-3
# Example
vagrant ssh worker-2

```


## Connecting to one of the cluster nodes

```sh
# To connect with one of the nodes, use the names c1-cp1, worker-1, worker-2, worker-3
# Example
vagrant ssh worker-2

```