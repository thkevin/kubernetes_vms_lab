# -*- mode: ruby -*-
# vi: set ft=ruby :

# LAB_BOX = "centos/8"
LAB_BOX = "centos/7"
CP_NODE_COUNT = 1
# Customizable vm names 
# Here, I use SNK wall names for fun :-)
# NODES_NAMES = ["maria", "rose", "sina"]
NODES_NAMES = ["worker-1", "worker-2", "worker-3"]
NODES_COUNT = NODES_NAMES.size

LAB_USER = "labkube"
LAB_GROUP = "labkube"
VAGRANT_DIR = File.dirname(__FILE__)
KEYS_DIR = "#{VAGRANT_DIR}/provisioning/files/keys"

Vagrant.require_version ">= 2.2.0"

Vagrant.configure("2") do |config|

  # Shared virtualbox settings
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  # Set lab_user as default user to connect to VMs
  VAGRANT_COMMAND = ARGV[0]
  if VAGRANT_COMMAND == "ssh"
    config.ssh.username = "#{LAB_USER}"
    config.ssh.private_key_path = "#{KEYS_DIR}/userkey"
    config.ssh.insert_key = 'true'
  end

  # Control panel node
  config.vm.define "cp-node" do |cp_node|
    cp_node.vm.box = LAB_BOX
    cp_node.vm.hostname = "c1-cp1"
    cp_node.vm.network  :private_network,
      virtualbox__intnet: "lab_network",
      ip: "172.16.94.10",
      netmask: "255.255.255.0",
      auto_config: true
    cp_node.vm.disk :disk, size: "60GB", primary: true
    cp_node.vm.provider "virtualbox" do |v|
      v.name = "c1-cp1"
    end
  end

  # Nodes
  (1..NODES_COUNT).each do |i|
    config.vm.define "worker-#{i}" do |worker|
      worker.vm.box = LAB_BOX
      worker.vm.hostname = "c1-node#{i}"
      worker.vm.network  :private_network,
        virtualbox__intnet: "lab_network",
        ip: "172.16.94.#{10 + i}",
        netmask: "255.255.255.0",
        auto_config: true
      worker.vm.disk :disk, size: "60GB", primary: true
      worker.vm.provider "virtualbox" do |v|
        v.name = NODES_NAMES[i - 1]
      end

      if i == NODES_COUNT
        worker.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          # Uncomment for verbose mode
          # ansible.verbose = "vvv"
          ansible.playbook = "provisioning/lab-node.yml"
          ansible.extra_vars = {
            lab_user: "#{LAB_USER}",
            lab_group: "#{LAB_GROUP}",
            keypair_dir: "#{KEYS_DIR}"
          }
        end
      end

    end
  end
end
