# RHADP Bootstrap

This repository contains "Infrastructure as Code (IaC)" for deploying the **Red Hat Automotive Development Platform (RHADP)** — a cloud-native development environment purpose-built for automotive software development.  

The setup automatically provisions multi-cloud OpenShift clusters (AWS, Azure, or GCP) with a **hybrid ARM/x86 architecture**, and installs a curated set of tools to accelerate the development of automotive applications and the **Red Hat In-Vehicle Operating System (RHIVOS)**.  

Upon deployment, the following core components are available out of the box:  
- **OpenShift Dev Spaces** – cloud-based developer workspaces  
- **OpenShift GitOps** – declarative management of infrastructure and applications  
- **OpenShift Pipelines** – CI/CD pipelines for building and testing automotive software  

The platform also installs following key services:  
- **cert-manager Operator for OpenShift** – automated TLS certificate management (via Let’s Encrypt)  
- **Red Hat build of Keycloak** – integrated identity and access management  

RHADP supports advanced use cases to address automotive-specific needs:  
- **ARM bare-metal node integration**  
- **OpenShift Virtualization** for running isolated virtual machines  
- **Building and testing RHIVOS images** directly in the cloud (software-in-the-loop)
- **Jumpstarter** for accessing automotive development boards from the cloud (hardware-in-the-loop)
 

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

## Contributing

Fork the repository and submit a pull request.

## Development

A list of ideas, open issues etc is [here](https://github.com/orgs/rhadp/projects/1). Also check the [Issues](https://github.com/rhadp/rhadp-bootstrap/issues) section of the this repository.

## Disclaimer

This is not an officially supported Red Hat product.