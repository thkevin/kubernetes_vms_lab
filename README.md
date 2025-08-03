# Vagrant setup to create a local kubernetes lab

This repository aims to quickly setup a kubernetes lab environment mainly for learning purposes.
The goal is to create a K8s cluster with this configuration:
1 control panel node
3 worker nodes

I created the setup of this [pluralsight course](https://app.pluralsight.com/library/courses/kubernetes-installation-configuration-fundamentals/table-of-contents).

The number of worker nodes is configurable but you should take into account your host resources.


## Requirements

### Host system requirements

* OS : Linux *I use pop OS 20.04*
* CPUs number : 12 (at least)
* RAM : 16GB (at least)
* Free disk space : about 250GB...

### Host packages requirements

The host must have installed :
* __Git__ to clone this repository (you can download and unzip too)
* __Vagrant__ to handle virtual machines
* __VirtualBox__ as Hypervisor
* __Ansible__ for provisionning

Last tests done with :

```
git version 2.34.1
Vagrant 2.4.7
Virtualbox 7.1
ansible [core 2.17.13]
```


*__Notes__*:

I use virtualbox as virtual machines hypervisor.
As I have disk space limitations on my desktop, I did the following setup via CLI to use my external drive as a disk space extension:

```sh
# Check the current default path for the VMs
vboxmanage list systemproperties | grep 'Default machine folder'

# Set a new path for the VMs to be created.
# Check the external drive(s) mount path with `df -h` or `fdisk -l`
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

## Before creating the cluster

Setup a keypair
```sh
# ssh-keygen a keypair in ansible files
# Get in provisioning file directory
cd provisioning/files/keys/

# Generate the user keypair
ssh-keygen -t ecdsa -b 521 -N "" -f ./userkey
```

## Create the cluster

The number of worker nodes is defined in the Vagrantfile with `NODES_COUNT = 3`.
Taking in account the resources of the host computer, this value can be edited before creating the cluster.

```sh
# Vagrant up!
vagrant up

# If for any transiant reason the VMs are created but the cluster not provsioned, vagrant provisiong can re run the provisioning part of the cluster creation
vagrant provision
```

## Access a node in the cluster

```sh
# To connect with one of the nodes, use the names c1-node, node-1, node-2, node-3
# Example on node 2
vagrant ssh node-2
```

## Ensure Kubernetes is running fine

```sh
# Access control plane node
vagrant ssh cp-node


# =================================================
#   Kubernetes commands (from the control plane)
# =================================================

# Get nodes
kubectl get nodes

# Get namespaces
kubectl get ns

# Get cluster endpoints
kubectl get endpoints kubernetes -n default -o wide
kubectl get endpoints kubernetes -o yaml

# Get kube system pods
kubectl get pod -n kube-system
kubectl get pod -n kube-system -o wide
kubectl get pod -n kube-system -o wide --watch

# Get all resources from all namespaces
kubectl get all --all-namespaces

# Get cluster infos
kubectl cluster-info

kubectl rollout restart ds/kube-proxy -n kube-system
kubectl logs pod-name-xxxx -c install-cni -n kube-system

# Debug calico
kubectl auth can-i create serviceaccounts/calico-cni-plugin -n calico-system --subresource token --as "system:serviceaccount:calico-system:calico-node"
```

## Destroy the cluster
This command will power off and destroy all the virtual machines of the cluster.

```sh
# Force with -f option
# vagrant destroy -f
vagrant destroy
```
