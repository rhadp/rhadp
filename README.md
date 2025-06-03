# RHADP Bootstrap

This repository provides infrastructure as code (IaC) for bootstrapping a Red Hat Automotive Development Platform (RHADP) cluster on AWS or Azure. 

The cluster is configured with a hybrid architecture featuring ARM control plane and x86 compute nodes, along with essential operators 
and configurations for development and production workloads.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Troubleshooting](#help)
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

AWS Account with appropriate permissions:
   - EC2 instances creation
   - VPC management
   - IAM roles and policies
   - Route53 (if using custom domain)

### Azure Prerequisites
- Azure subscription with appropriate permissions
- Azure credentials configured
- Sufficient Azure quotas for:
  - Virtual Machines
  - Virtual Networks
  - Public IPs
  - Load Balancers

### Required Credentials
- Red Hat pull secret (download from [Red Hat Console](https://console.redhat.com))
- Cloud provider credentials (AWS/Azure)
- Optional: Custom domain certificates if using custom domain


## Architecture

The RHADP cluster is designed with a hybrid architecture:

### Control Plane
- ARM-based nodes for improved cost efficiency
- High availability configuration
- Dedicated nodes for control plane components

### Compute Nodes
- x86-based nodes for maximum compatibility
- Separate node pools for:
  - Application workloads
  - Infrastructure components
  - Monitoring and logging


## Installation

### Repository Setup

1. Clone this repository:
```bash
git clone <repository-url>
cd rhadp-bootstrap
```

2. Create a Python virtual environment (recommended):
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### Inventory Setup

1. Copy the inventory template:
```bash
cp inventory/main.yml.example inventory/main.yml
```

2. Configure your environment:
```bash
# Edit the inventory file with your specific settings
vi inventory/main.yml
```

Required inventory variables:
- `cluster_name`: Unique name for your cluster
- `base_domain`: Base domain for the cluster
- `cloud_provider`: AWS or Azure
- `region`: Cloud provider region
- `pull_secret`: Red Hat pull secret
- `ssh_key`: SSH public key for node access

### Cluster Bootstrap

1. Initialize the cluster:
```bash
ansible-playbook -i inventory/ 1_bootstrap.yml
```

2. Monitor the installation:
```bash
# View installation logs
tail -f $HOME/.openshift/<cluster-name>/.openshift-install.log

# Check cluster status
oc get clusteroperators
```

## Usage

### Common Operations

1. Start the cluster:
```bash
ansible-playbook -i inventory/ start.yml
```

2. Stop the cluster:
```bash
ansible-playbook -i inventory/ stop.yml
```

3. Destroy the cluster:
```bash
ansible-playbook -i inventory/ 99_destroy.yml
```

## Help

- Check the [OpenShift Documentation](https://docs.openshift.com)
- Review [Red Hat Knowledge Base](https://access.redhat.com)
- Open an issue in this repository
- Contact Red Hat Support

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request


## References
- [OpenShift Installation on AWS](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/installing_on_aws/index)
- [Post-installation Configuration](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/postinstallation_configuration/index)

    