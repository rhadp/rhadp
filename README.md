# RHADP Bootstrap

This repository provides infrastructure as code (IaC) for bootstrapping a Red Hat Automotive Development Platform (RHADP) cluster on AWS or Azure. 

The cluster is configured with a hybrid architecture featuring ARM control plane and x86 compute nodes, along with essential operators 
and configurations for development and production workloads.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [References](#references)

## Prerequisites

### Required Software Versions
- OpenShift CLI (`oc`) v4.18 or later
- Python 3.9 or later with pip
- Ansible 2.15 or later
- Git
- AWS CLI v2 (for AWS deployments)
- Azure CLI (for Azure deployments)

### Required Credentials
- Red Hat pull secret (download from [Red Hat Console](https://console.redhat.com))
- Cloud provider credentials (AWS/Azure)


## Architecture

The RHADP cluster is designed with a hybrid architecture:

### Control Plane
- ARM-based nodes for improved cost efficiency
- High availability configuration
- Dedicated nodes for control plane components

### Compute Nodes
- x86-based nodes for maximum compatibility
- ARM-based nodes for building the Red Hat In-Vehicle OS (RHIVOS)
- ARM-bare-metal nodes to run RHIVOS VMs on the cluster (optional)
- x86-based nodes with GPU acceleration for training and inference workloads (optional)


## Installation

### Repository Setup

Clone this repository:
```bash
git clone https://github.com/rhadp/rhadp-bootstrap.git
cd rhadp-bootstrap
```

Create a Python virtual environment (recommended):
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
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

Required inventory variables:
- `cluster_name`: Unique name for your cluster
- `cluster_domain`: Base domain for the cluster
- `cloud_provider`: AWS or Azure
- `pull_secret_file`: path to the OpenShift pull secret file
- `cluster_ssh_key`: location of the private key
- `aws credentials` or `azure credentials`


### Cluster Bootstrap

Initialize the cluster:
```bash
ansible-playbook -i inventory/ 1_bootstrap.yml
```

Monitor the installation:
```bash
tail -f $HOME/.openshift/<cluster-name>/.openshift-install.log
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


## References
- [OpenShift Installation on AWS](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/installing_on_aws/index)
- [Post-installation Configuration](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/postinstallation_configuration/index)

    