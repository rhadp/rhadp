# Red Hat Automotive Development Platform

## Overview


## Installation

### Repository Setup

Clone the repository:
```bash
git clone https://github.com/rhadp/rhadp-bootstrap.git
cd rhadp-bootstrap
```

Create a Python virtual environment (recommended):
```bash
python3.12 -m venv venv
source venv/bin/activate

# install the dependencies
pip install -r requirements.txt

# install the azure collection
ansible-galaxy collection install azure.azcollection --force
pip install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements.txt
```

### Inventory Setup

Copy the inventory template:
```bash
cp inventory/main.yml.example inventory/main.yml
```

Configure your environment:
```bash
# Edit the inventory file with your specific settings
vi inventory/main.yml
```

### Cluster Bootstrap

Initialize the cluster:
```bash
ansible-playbook -i inventory/ 0_bootstrap_all.yml
```

Monitor the installation process:
```bash
tail -f $HOME/.openshift/<cluster-name>-<cloud-provider>/.openshift-install.log
```

Destroy the cluster:
```bash
ansible-playbook -i inventory/ 99_destroy_cluster.yml
```
