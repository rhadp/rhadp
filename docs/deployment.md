## Depployment Guide

This document is outlines the basic steps to deploy the Red Hat Automotive Development Platform (RHADP). 

It is the TL;DR version. For in-depth documentation, start [here](README.md).

### Repository setup

Clone the repository:
```bash
git clone https://github.com/rhadp/rhadp.git
cd rhadp
```

In case you want to make changes to the Ansible playbooks, roles or to any of the configuration templates, `fork` the repository, instead of just cloning it.

### Environment preparation

Create a Python virtual environment, install Ansible and its dependencies:

```bash
python3.12 -m venv venv
source venv/bin/activate

# install the dependencies
pip install -r requirements.txt

# install the azure collection
ansible-galaxy collection install azure.azcollection --force
pip install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements.txt
```

### Inventory setup

Folder `inventory` contains examples of different cluster configurations. Select a configuration example (e.g. inventory/cluster.yml.example) and modify it to your needs.

Copy the inventory template:
```bash
cp inventory/cluster.yml.example inventory/cluster.yml
```

Edit the inventory file with your specific settings:
```bash
vi inventory/cluster.yml
```

### Cluster bootstrap

To deploy the OpenShift cluster with all enabled features, run the following playbook:

```bash
ansible-playbook -i inventory/ 0_all.yml
```
**NOTE:** Depending on the cluster topology, this will take up to 90 minutes!

To monitor the installation process:
```bash
tail -f $HOME/.openshift/<cluster-name>-<cloud-provider>/.openshift-install.log
```

### Cluster lifecycle

In order to start and stop the cluster, you can use the following playbooks:
```bash
# start the cluster
ansible-playbook -i inventory/ start.yml

# stop the cluster
ansible-playbook -i inventory/ stop.yml
```

In order to destroy the cluster, use the following playbook:
```bash
ansible-playbook -i inventory/ 9_destroy_cluster.yml
```
