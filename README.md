# RHADP Bootstrap

This repository provides infrastructure as code (IaC) for bootstrapping a Red Hat Automotive Development Platform (RHADP) cluster on AWS or Azure. 

The cluster is configured with a hybrid architecture featuring ARM control plane and x86 compute nodes, along with essential operators 
and configurations for development and production workloads.

## Prerequisites

Before using this repository, ensure you have the following prerequisites:

AWS Account with appropriate permissions:
   - EC2 instances creation
   - VPC management
   - IAM roles and policies
   - Route53 (if using custom domain)

Required Software:
   - OpenShift CLI (`oc`)
   - Kubernetes CLI (`kubectl`)
   - Python 3.x with pip
   - Ansible
   - Git

Credentials:
   - AWS credentials with appropriate permissions
   - Red Hat pull secret


## Cluster creation

### Repository Setup

Clone this repository:

```bash
git clone <repository-url>
cd rhadp-bootstrap
```

### Inventory setup

Copy the inventory template to a new file:

```bash
cp inventory/main.yml.example inventory/main.yml
```

Edit the inventory file to match your environment:

```bash
vi inventory/main.yml
```

### Cluster bootstrap

Initialize the cluster:

```bash
ansible-playbook -i inventory/ 1_bootstrap.yml
```

Monitor cluster installation:

```bash
tail -f $HOME/.openshift/<cluster-name>/.openshift-install.log
```


## References
- [OpenShift Installation on AWS](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/installing_on_aws/index)
- [Post-installation Configuration](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/postinstallation_configuration/index)
- [Cert-Manager Operator](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/security_and_compliance/cert-manager-operator-for-red-hat-openshift)
    