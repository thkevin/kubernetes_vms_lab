# Vagrant setup to create a local kubernetes lab

This repository aims to quickly setup a kubernetes lab environment mainly for learning purposes.
The goal is to create a K8s cluster with this configuration:
1 control panel node
3 worker nodes

I created the setup of this [pluralsight course](https://app.pluralsight.com/library/courses/kubernetes-installation-configuration-fundamentals/table-of-contents).

The number of worker nodes is configurable but you should take into account your host resources.


## Requirements

### Host packages requirements
The host must have :
* __Git__ to clone this repository (you can download and unzip too)
* __Vagrant__ to handle virtual machines
* __VirtualBox__ as Hypervisor
* __Ansible__ for provisionning

### Host system requirements
* OS : Linux *I use pop OS 20.04*
* CPUs number : 12 (at least)
* RAM : 16GB (at least)
* Free disk space : about 250GB...

*__Notes__*:

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

## Clone the repo
```sh
# Clone the repo and change directory
git clone git@github.com:thkevin/kubernetes_vms_lab.git

# Get into the project directory
cd kubernetes_vms_lab
```

## Create the cluster
```sh
# Vagrant up!
vagrant up
```

## Access a node in the cluster

```sh
# To connect with one of the nodes, use the names c1-cp1, worker-1, worker-2, worker-3
# Example 1
vagrant ssh node-2

# Example 2
vagrant ssh cp-node
```

## Destroy the cluster
This command will power off and destroy all the virtual machines of the cluster.

```sh
vagrant destroy [-f]
```