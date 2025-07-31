# RHADP Bootstrap

This repository provides infrastructure as code (IaC) for bootstrapping a Red Hat Automotive Development Platform (RHADP) cluster on AWS, Azure or GCP. 

The cluster is configured with a hybrid architecture featuring ARM control plane and x86/ARM compute nodes, along with essential operators 
and configurations for development and production workloads.


## Installation

### Repository Setup

Clone this repository:
```bash
git clone https://github.com/rhadp/rhadp-bootstrap.git
cd rhadp-bootstrap
```

Create a Python virtual environment (recommended):
```bash
python3.12 -m venv venv
source venv/bin/activate
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
ansible-playbook -i inventory/ 1_bootstrap.yml
```

Monitor the installation process:
```bash
tail -f $HOME/.openshift/<cluster-name>-<cloud-provider>/.openshift-install.log
```

## Usage

### Common Operations

Start the cluster:
```bash
ansible-playbook -i inventory/ start.yml
```

Stop the cluster:
```bash
ansible-playbook -i inventory/ stop.yml
```

Destroy the cluster:
```bash
ansible-playbook -i inventory/ 99_destroy.yml
```

## Contributing

Fork the repository and submit a pull request.

## Development

A list of ideas, open issues etc is [here](docs/to.md). Also check the [Issues](https://github.com/rhadp/rhadp-bootstrap/issues) section of the this repository.
