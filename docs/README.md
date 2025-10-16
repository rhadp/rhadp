# Red Hat Automotive Development Platform - Technical Documentation

## Table of Contents

1. [Introduction](#1-introduction)
2. [Technologies and Components](#2-technologies-and-components)
3. [Prerequisites](#3-prerequisites)
4. [Deployment Guide](#4-deployment-guide)
5. [Configuration Reference](#5-configuration-reference)
6. [Multi-Cloud Deployment](#6-multi-cloud-deployment)
7. [Multi-Architecture Support](#7-multi-architecture-support)
8. [Platform Services](#8-platform-services)
9. [Cluster Management](#9-cluster-management)
10. [Security Considerations](#10-security-considerations)
11. [Reference](#11-reference)
12. [Appendices](#appendices)

---

## 1. Introduction

### 1.1 What is RHADP?

The Red Hat Automotive Development Platform (RHADP) is a comprehensive Infrastructure as Code (IaC) solution that automates the deployment of a complete cloud-native development environment specifically designed for automotive software development. RHADP provides DevOps engineers with a turnkey platform for building, testing, and deploying automotive applications and the Red Hat In-Vehicle Operating System (RHIVOS).

### 1.2 Purpose and Use Cases

RHADP is designed to support:

- **Automotive Software Development**: Cloud-native development workflows for modern automotive applications
- **RHIVOS Development**: Complete development environment for Red Hat In-Vehicle Operating System
- **Multi-Architecture Development**: Support for ARM and x86 workloads in automotive contexts
- **Hardware Testing**: Integration with real and virtual automotive hardware through Jumpstarter
- **CI/CD Automation**: Pre-configured pipelines for automotive software build, test, and deployment

### 1.3 Key Features

- **Multi-Cloud Support**: Deploy on AWS, Azure, or GCP with identical workflows
- **Multi-Architecture**: Native support for x86_64, aarch64 (ARM64), and hybrid architectures
- **Automated Deployment**: Single-command deployment of complete OpenShift platform
- **Pre-Integrated Toolchain**: GitOps, Pipelines, Dev Spaces, and testing tools out-of-the-box
- **Hardware Testing**: Jumpstarter integration for automated testing on real and virtual hardware
- **Lifecycle Management**: Start, stop, reconfigure, and destroy clusters with Ansible playbooks

### 1.4 Architecture Overview

RHADP deploys a layered architecture consisting of:

**Infrastructure Layer**
- OpenShift Container Platform 4.19
- Cloud provider infrastructure (AWS, Azure, or GCP)
- Multi-architecture compute nodes (x86_64, ARM64, or hybrid)
- OpenShift Virtualization (optional, for VM workloads)

**Platform Services Layer**
- Identity and Access Management (Keycloak + OAuth)
- Certificate Management (cert-manager with Let's Encrypt)
- GitOps declarative management (OpenShift GitOps/ArgoCD)
- CI/CD pipelines (OpenShift Pipelines/Tekton)

**Development Tools Layer**
- OpenShift Dev Spaces (browser-based IDE)
- Developer Hub (optional, service catalog and portal)
- Jumpstarter (optional, hardware testing automation)
- GitHub/GitLab runner integration

---

## 2. Technologies and Components

### 2.1 Core Infrastructure

#### OpenShift Container Platform 4.19
- Enterprise Kubernetes distribution for container orchestration
- Application platform for deploying containerized workloads
- Support for multiple deployment topologies (default, compact, bare-metal)
- Built-in monitoring, logging, and observability

#### OpenShift Virtualization (Optional)
- Virtual machine management alongside container workloads
- Based on KubeVirt technology
- Unified management of VMs and containers
- Requires bare-metal nodes with nested virtualization support

#### cert-manager Operator for OpenShift
- Automated X.509 certificate management
- Let's Encrypt integration for automatic TLS certificate provisioning
- Certificate lifecycle management (issuance, renewal, revocation)
- Support for ingress and route certificate automation

### 2.2 Identity and Access Management

#### Red Hat build of Keycloak
- Enterprise-grade identity and access management solution
- OAuth 2.0 and OpenID Connect (OIDC) provider
- User federation and single sign-on (SSO) capabilities
- Integration with external identity providers (GitHub, GitLab, LDAP)

#### htpasswd OAuth Provider
- Local user authentication mechanism
- File-based user credential storage
- Admin and service account management
- Bootstrap authentication before external providers are configured

### 2.3 Development Platform

#### OpenShift Dev Spaces
- Cloud-based developer workspaces accessible via web browser
- Based on Eclipse Che technology
- Pre-configured development environments for consistent team workflows
- Support for devfile-based workspace definitions
- Integrated terminal, debugger, and Git tools

#### Developer Hub (Optional)
- Unified developer portal powered by Red Hat Developer Hub
- Service catalog for discovering and using platform services
- API documentation and technical resources
- Team collaboration and project templates

#### OpenShift GitOps (ArgoCD)
- Declarative infrastructure and application management
- GitOps workflows for continuous delivery
- Automated synchronization between Git repositories and cluster state
- Multi-cluster and multi-tenant support
- Rollback and drift detection capabilities

#### OpenShift Pipelines (Tekton)
- Cloud-native CI/CD pipeline framework
- Kubernetes-native build automation
- Reusable pipeline tasks and components
- Integration with Git webhooks for automated builds
- Support for parallel execution and complex workflows

### 2.4 Testing and Automation

#### Jumpstarter (Optional)
- Automated testing framework for real and virtual hardware
- CI/CD integration for hardware-in-the-loop testing
- QEMU exporter support for virtual device testing
- Remote device management and orchestration
- Ideal for automotive ECU and embedded system testing

### 2.5 Infrastructure as Code

#### Ansible
- Automation engine for deployment and configuration management
- Role-based modular architecture for extensibility
- Idempotent playbook execution
- Support for cloud provider APIs (AWS, Azure, GCP)

#### OpenShift Installer (openshift-install)
- Official Red Hat installer for OpenShift cluster bootstrapping
- Installer-provisioned infrastructure (IPI) mode
- Multi-cloud provider support
- Declarative cluster configuration via install-config.yaml

---

## 3. Prerequisites

### 3.1 System Requirements

#### Local Workstation Requirements
- **Operating System**: macOS, Linux, or Windows with WSL2
- **Python**: Version 3.12 or later
- **Git**: Version 2.0 or later

### 3.2 Cloud Provider Requirements

#### 3.2.1 AWS Requirements

**Reference**: [Installing on AWS - Red Hat OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/installing_on_aws/index)

**Account and Permissions**
- AWS account with programmatic access (access key ID and secret access key)
- IAM permissions for resource creation:
  - EC2 instances, VPCs, subnets, security groups
  - EBS volumes and snapshots
  - Elastic Load Balancers (ELB/ALB)
  - Route 53 DNS records (if using AWS DNS)
  - S3 buckets (for registry storage)
  - IAM roles and instance profiles

**Service Quotas and Limits**
- EC2 instances: Sufficient quota for control plane (3x) and worker nodes (3x minimum)
- VPC resources: 1 VPC, multiple subnets across availability zones
- Elastic IPs: 2-3 for load balancers
- EBS volumes: General Purpose SSD (gp3) quota
- Regional vCPU limits: Check for m8g, m6g, or m6a instance families

**DNS Configuration**
- Route 53 hosted zone (recommended) for cluster domain
- OR external DNS provider with ability to create DNS records
- Ability to delegate a subdomain for cluster use

**Additional Considerations**
- Support for ARM instances (Graviton2/3) in selected regions
- Bare-metal instance availability (e.g. m6g.metal) if using OpenShift Virtualization
- Availability zone requirements: 3 AZs recommended for high availability

#### 3.2.2 Azure Requirements

**Reference**: [Installing on Azure - Red Hat OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_azure/index)

**Account and Permissions**
- Azure subscription with appropriate resource limits
- Service principal or managed identity with Contributor role on subscription
- Azure Active Directory permissions for app registration

**Service Principal Setup**
```bash
# Create service principal for OpenShift
az ad sp create-for-rbac --name openshift-sp --role Contributor \
  --scopes /subscriptions/<subscription-id>
```

**Service Quotas and Limits**
- Virtual Machines: Quota for Standard_D8s_v5 or equivalent (control plane and workers)
- Storage Accounts: Premium LRS storage quota
- Virtual Networks: 1 VNet with multiple subnets
- Load Balancers: Standard SKU load balancer quota
- Public IP addresses: Multiple public IPs for ingress and API

**DNS Configuration**
- Azure DNS zone (recommended) for cluster domain
- OR external DNS provider with ability to create DNS records
- DNS zone must be in same subscription or accessible via service principal

**Additional Considerations**
- ARM instance support: Standard_D8ps_v5 and similar Ampere-based VMs
- Regional availability: Verify ARM VM availability in target region
- Network security: Default outbound access considerations (Azure retiring default outbound after Sept 2025)

#### 3.2.3 GCP Requirements

**Reference**: [Installing on GCP - Red Hat OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_gcp/index)

**Account and Permissions**
- GCP project with billing enabled
- Service account with required IAM roles:
  - Compute Admin
  - Service Account Admin
  - Service Account User
  - Storage Admin
  - DNS Administrator (if using Cloud DNS)

**Service Account Setup**
```bash
# Create service account
gcloud iam service-accounts create openshift-sa \
  --display-name "OpenShift Service Account"

# Grant required roles
gcloud projects add-iam-policy-binding <project-id> \
  --member "serviceAccount:openshift-sa@<project-id>.iam.gserviceaccount.com" \
  --role "roles/compute.admin"

# Create and download key
gcloud iam service-accounts keys create osServiceAccount.json \
  --iam-account openshift-sa@<project-id>.iam.gserviceaccount.com
```

**API Services (Must be Enabled)**
- Compute Engine API
- Cloud Resource Manager API
- Google DNS API (if using Cloud DNS)
- IAM Service Account Credentials API
- Service Management API
- Service Usage API
- Google Cloud APIs
- Identity and Access Management (IAM) API

**Service Quotas and Limits**
- Compute Engine: Quota for t2a-standard-8 instances (ARM) or equivalent
- Persistent Disks: SSD persistent disk quota
- Networks: 1 VPC network with custom subnets
- Load Balancers: Network load balancer quota
- In-use IP addresses: Multiple external IPs

**DNS Configuration**
- Cloud DNS managed zone (recommended) for cluster domain
- OR external DNS provider with ability to create DNS records

**Additional Considerations**
- ARM support: T2A instance family (Ampere Altra) availability by region
- Confidential computing: AMD SEV and Intel TDX support in OpenShift 4.19
- Regional quotas: Verify quota availability in target region

### 3.3 Red Hat Account

**Required Account**
- Red Hat account (free or with active subscription)

**Obtaining Pull Secret**
1. Navigate to [Red Hat OpenShift Cluster Manager](https://console.redhat.com/openshift/create)
2. Log in with your Red Hat account credentials
3. Click "Create Cluster"
4. Select "Datacenter" tab
5. Click "Download pull secret" button
6. Save the pull secret file (JSON format) for use in inventory configuration

**Why Pull Secret is Required**
- Authentication to Red Hat container registries (quay.io, registry.redhat.io)
- Access to official Red Hat container images
- Operator catalog access for Red Hat operators
- Support entitlement verification (for subscribed accounts)

### 3.4 Additional Requirements (Optional)

#### GitHub Integration (Optional)
- GitHub account (personal or organization)
- GitHub Personal Access Token (PAT) with scopes:
  - `repo` (full control of private repositories)
  - `admin:org` (for organization integration)
  - `workflow` (for GitHub Actions integration)
- GitHub OAuth App (for Keycloak integration):
  - Client ID and Client Secret
  - Authorization callback URL: `https://keycloak-<namespace>.apps.<cluster_domain>/auth/realms/master/broker/github/endpoint`

#### GitLab Integration (Optional)
- GitLab account (gitlab.com or self-hosted)
- GitLab Personal Access Token with scopes:
  - `api` (access to GitLab API)
  - `read_repository` and `write_repository`
- GitLab OAuth Application (for Keycloak integration)

#### Let's Encrypt
- Valid email address for certificate expiration notifications
- Email must be accessible for emergency certificate issues
- Recommended: Use a team or group email, not personal

---

## 4. Deployment Guide

### 4.1 Environment Setup

#### 4.1.1 Clone the Repository

```bash
git clone https://github.com/rhadp/rhadp.git
cd rhadp
```

If you plan to customize the Ansible playbooks, roles, or configuration templates, fork the repository instead of cloning it directly.

#### 4.1.2 Setup Python Virtual Environment

```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # On macOS/Linux
# venv\Scripts\activate   # On Windows with PowerShell

# Install Python dependencies
pip install -r requirements.txt

# Install Azure Ansible collection (required only if using Azure)
ansible-galaxy collection install azure.azcollection --force
pip install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements.txt
```

#### 4.1.3 Verify Ansible Installation

```bash
# Check Ansible version (should be 2.16 or later)
ansible --version

# Verify installed collections
ansible-galaxy collection list

# Expected collections include:
# - amazon.aws
# - google.cloud
# - community.general
# - kubernetes.core
```

### 4.2 Configuration

#### 4.2.1 Copy Example Configuration

```bash
cp inventory/main.yml.example inventory/main.yml
```

#### 4.2.2 Obtain Red Hat Pull Secret

1. Visit [Red Hat OpenShift Cluster Manager](https://console.redhat.com/openshift/create)
2. Log in with your Red Hat account
3. Click "Datacenter" tab
4. Download the pull secret (JSON file)
5. Save to `inventory/pull-secret.txt`:

```bash
# Save pull secret to inventory directory
cat > inventory/pull-secret.txt <<'EOF'
<paste your pull secret JSON here>
EOF
```

#### 4.2.3 Configure Cloud Provider Credentials

Edit `inventory/main.yml` and uncomment the appropriate cloud provider section:

**For AWS:**
```yaml
cloud_provider: "aws"

aws_access_key_id: "AKIAIOSFODNN7EXAMPLE"
aws_secret_access_key: "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
aws_default_region: "us-east-2"
```

**For Azure:**
```yaml
cloud_provider: "azure"

azure_guid: "00000000-0000-0000-0000-000000000000"
azure_client_id: "00000000-0000-0000-0000-000000000000"
azure_password: "your-service-principal-password"
azure_tenant: "your-tenant.onmicrosoft.com"
azure_subscription: "00000000-0000-0000-0000-000000000000"
azure_resource_group: "rhadp-rg"
azure_default_region: "eastus"
```

**For GCP:**
```yaml
cloud_provider: "gcp"

gcp_service_account_key_file: "$HOME/.gcp/osServiceAccount.json"
gcp_service_account_email: "openshift-sa@your-project.iam.gserviceaccount.com"
gcp_project_id: "your-project-id"
gcp_organization_id: "123456789012"
gcp_default_region: "us-central1"
```

#### 4.2.4 Configure Cluster Settings

Edit `inventory/main.yml` with your cluster configuration:

```yaml
# Basic cluster configuration
cluster_name: "rhadp"
cluster_domain: "rhadp.example.com"
cluster_toplevel_domain: "example.com"
cluster_version: "4.19.14"

# Architecture configuration
cluster_arch: "x86_64"  # Options: x86_64, aarch64, multi
control_plane_arch: "amd64"  # Only relevant when cluster_arch=multi

# Topology
cluster_topology: "default"  # Options: default, compact, metal

# Storage
cluster_default_storage_class: "gp3-csi"  # AWS: gp3-csi, Azure: managed-csi, GCP: standard-csi

# Installer architecture (for your local machine)
installer_architecture: "mac-arm64"  # Options: mac, mac-arm64, linux, linux-arm64
```

#### 4.2.5 Configure Advanced Options

**Multi-Architecture and Virtualization:**
```yaml
# Add ARM worker nodes to x86 cluster
setup_arm_worker_nodes: true

# Add bare-metal worker node (AWS only, for virtualization)
setup_baremetal_worker_node: true

# Install OpenShift Virtualization (requires bare-metal node)
setup_virtualization: true
```

**Platform Services:**
```yaml
# Deploy optional platform components
rhadp_deploy_developer_hub: true
rhadp_deploy_jumpstart: true

# GitOps repository configuration
rhadp_gitops_repo_url: "https://github.com/rhadp/rhadp-manifests"
rhadp_gitops_repo_revision: "main"
```

**Certificate Management:**
```yaml
# Let's Encrypt email for certificate notifications
letsencrypt_email: "devops-team@example.com"
```

**GitHub Integration (Optional):**
```yaml
github_endpoint: "https://github.com"
github_org: "your-organization"
github_api_pat: "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
github_oauth_client_id: "Iv1.xxxxxxxxxxxxxxxx"
github_oauth_client_secret: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

**Security Settings:**
```yaml
# Change default admin credentials
default_admin_user: "admin"
default_admin_password: "ChangeMe123!"
default_admin_email: "admin@example.com"

# Security hardening
remove_kubeadmin: true  # Remove default kubeadmin user
remove_selfprovisioning: true  # Prevent users from creating projects

# Keycloak credentials
keycloak_db_user: "keycloak_admin"
keycloak_db_password: "SecurePassword123!"
keycloak_client_secret: "AnotherSecureSecret456!"
```

### 4.3 Deploy the Platform

#### 4.3.1 Run Full Deployment

```bash
# Activate virtual environment if not already active
source venv/bin/activate

# Run complete deployment playbook
ansible-playbook -i inventory/ 0_all.yml
```

This single command will:
1. Gather cluster facts and validate configuration
2. Download OpenShift installer binary
3. Generate SSH keys for cluster nodes
4. Create cloud provider infrastructure
5. Bootstrap OpenShift cluster (control plane + workers)
6. Configure cluster topology (ARM nodes, bare-metal nodes)
7. Install cert-manager and configure Let's Encrypt
8. Deploy and configure Keycloak for authentication
9. Install OpenShift GitOps and Pipelines
10. Deploy platform services (Dev Spaces, Developer Hub, Jumpstarter)
11. Configure RBAC and admin users

#### 4.3.2 Deployment Timeline

**Expected Duration: 90-120 minutes**

Breakdown by phase:
- **Cluster Bootstrap (45-60 min)**: OpenShift installer provisions infrastructure and control plane
- **Cluster Configuration (10-15 min)**: Additional worker nodes, machine sets, CNV operator
- **Operator Installation (20-30 min)**: cert-manager, Keycloak, GitOps, Pipelines operators
- **Platform Services (15-25 min)**: Dev Spaces, Developer Hub, Jumpstarter deployment

**Factors Affecting Duration:**
- Cloud provider region and resource availability
- Number of worker nodes
- Network bandwidth for container image pulls
- ARM vs x86 architecture (ARM may be slower in some regions)

#### 4.3.3 Monitor Deployment Progress

**Watch Ansible Output:**
```bash
# Ansible playbook provides real-time task execution status
# Look for TASK names and OK/CHANGED/FAILED status
```

**Monitor OpenShift Installer Logs:**
```bash
# Tail installer logs during cluster bootstrap
tail -f $HOME/.openshift/<cluster-name>-<cloud-provider>/.openshift_install.log

# Example:
tail -f $HOME/.openshift/rhadp-aws/.openshift_install.log
```

**Check Cloud Provider Console:**
- AWS: EC2 Console to view instances, VPCs, load balancers
- Azure: Resource Groups to view VMs, networks, load balancers
- GCP: Compute Engine to view instances, networks, load balancers

**Common Log Messages:**
- `Waiting for bootstrap complete`: Control plane is initializing (15-20 min)
- `Waiting for cluster operators`: Cluster operators are deploying (10-15 min)
- `Cluster is initialized`: Bootstrap complete, cluster operational

#### 4.3.4 Deployment Troubleshooting

**If Deployment Fails:**

1. **Check installer logs** for detailed error messages:
   ```bash
   cat $HOME/.openshift/<cluster-name>-<cloud-provider>/.openshift_install.log
   ```

2. **Verify cloud provider credentials** are correct in `inventory/main.yml`

3. **Check service quotas** in cloud provider console

4. **Verify pull secret** is valid and properly formatted

5. **Review Ansible playbook output** for failed tasks

**Retry Deployment:**
```bash
# If deployment fails, destroy partial resources first
ansible-playbook -i inventory/ 9_destroy_cluster.yml

# Then retry full deployment
ansible-playbook -i inventory/ 0_all.yml
```

### 4.4 Access the Platform

#### 4.4.1 Cluster Access URLs

Once deployment is complete, access the platform at:

**OpenShift Web Console:**
```
https://console-openshift-console.apps.<cluster_name>.<cluster_domain>
```
Example: `https://console-openshift-console.apps.rhadp.rhadp.example.com`

**OpenShift API Endpoint:**
```
https://api.<cluster_name>.<cluster_domain>:6443
```
Example: `https://api.rhadp.rhadp.example.com:6443`

**Kubeconfig Location:**
```
$HOME/.openshift/<cluster-name>-<cloud-provider>/auth/kubeconfig
```
Example: `$HOME/.openshift/rhadp-aws/auth/kubeconfig`

#### 4.4.2 Default Credentials

**Admin User:**
- Username: `admin` (or value of `default_admin_user`)
- Password: `openshift` (or value of `default_admin_password`)

**IMPORTANT SECURITY NOTICE:**
Change the default admin password immediately after first login:
1. Log in to OpenShift Console with admin credentials
2. Navigate to User Management → Users
3. Update admin user password

#### 4.4.3 Platform Service URLs

**OpenShift Dev Spaces:**
```
https://devspaces.apps.<cluster_name>.<cluster_domain>
```

**Keycloak Admin Console:**
```
https://keycloak-<namespace>.apps.<cluster_name>.<cluster_domain>
```
Example: `https://keycloak-rhadp-auth.apps.rhadp.rhadp.example.com`

**OpenShift GitOps (ArgoCD):**
```
https://argocd-server-openshift-gitops.apps.<cluster_name>.<cluster_domain>
```

**Developer Hub (if deployed):**
```
https://developer-hub-<namespace>.apps.<cluster_name>.<cluster_domain>
```

#### 4.4.4 CLI Access

**Using OpenShift CLI (oc):**

```bash
# Set KUBECONFIG environment variable
export KUBECONFIG=$HOME/.openshift/<cluster-name>-<cloud-provider>/auth/kubeconfig

# Example:
export KUBECONFIG=$HOME/.openshift/rhadp-aws/auth/kubeconfig

# Verify authentication
oc whoami
# Output: system:admin

# Login as admin user (htpasswd)
oc login -u admin -p openshift

# Verify login
oc whoami
# Output: admin
```

**Download oc CLI:**
```bash
# Download from OpenShift mirror
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.19.14/openshift-client-linux.tar.gz

# Extract
tar -xzf openshift-client-linux.tar.gz

# Move to PATH
sudo mv oc kubectl /usr/local/bin/

# Verify installation
oc version
```

### 4.5 Verify Deployment

Run the following commands to verify successful deployment:

```bash
# Set KUBECONFIG
export KUBECONFIG=$HOME/.openshift/<cluster-name>-<cloud-provider>/auth/kubeconfig

# Check authentication
oc whoami

# List all cluster nodes
oc get nodes
# Expected: 3 control plane nodes, 3+ worker nodes

# Check node status and architecture
oc get nodes -o wide
# Look for Ready status and aarch64/amd64 architecture

# Verify cluster operators are available
oc get co
# All operators should show AVAILABLE=True, PROGRESSING=False, DEGRADED=False

# Check all pods across namespaces
oc get pods -A

# Verify platform components
oc get pods -n rhadp-devspaces
oc get pods -n openshift-gitops
oc get pods -n openshift-pipelines

# Check ingress certificates (should be Let's Encrypt)
oc get certificate -A

# Verify Keycloak deployment
oc get pods -n rhadp-auth  # or your auth namespace

# Check ArgoCD applications
oc get applications -n openshift-gitops

# Review cluster version and update channel
oc get clusterversion
```

**Expected Healthy State:**
- All nodes in `Ready` status
- All cluster operators `AVAILABLE=True`, `DEGRADED=False`
- All pods in `Running` or `Completed` status
- Certificates issued and valid
- ArgoCD applications synced

---

## 5. Configuration Reference

This section provides a complete reference for all configuration variables available in `inventory/main.yml`.

### 5.1 Cluster Configuration (Part 1)

#### 5.1.1 Cloud Provider and Version

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cloud_provider` | String | `aws` | Yes | Cloud platform: `aws`, `azure`, `gcp` |
| `cluster_version` | String | `4.19.14` | Yes | OpenShift version (must match available version on Red Hat mirror) |
| `installer_architecture` | String | `mac-arm64` | Yes | Local machine architecture: `mac`, `mac-arm64`, `linux`, `linux-arm64` |

**Example:**
```yaml
cloud_provider: "aws"
cluster_version: "4.19.14"
installer_architecture: "linux"  # For x86_64 Linux workstation
```

#### 5.1.2 Cluster Architecture

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cluster_arch` | String | `x86_64` | Yes | Node architecture: `x86_64` (all x86), `aarch64` (all ARM), `multi` (hybrid, AWS/Azure only) |
| `control_plane_arch` | String | `amd64` | Conditional | Control plane architecture (only when `cluster_arch=multi`): `amd64`, `arm64` |

**Example:**
```yaml
cluster_arch: "multi"           # Hybrid cluster
control_plane_arch: "amd64"     # x86 control plane, ARM workers possible
```

#### 5.1.3 Cluster Topology

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cluster_topology` | String | `default` | Yes | Deployment pattern: `default` (3 control + workers), `compact` (control nodes also run workloads), `metal` (bare-metal) |

**Example:**
```yaml
cluster_topology: "default"
```

#### 5.1.4 Cluster Naming and DNS

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cluster_name` | String | `rhadp` | Yes | Cluster identifier (DNS-compatible: lowercase, alphanumeric, hyphens, max 253 chars) |
| `cluster_domain` | String | `rhadp.example.com` | Yes | Base FQDN for cluster routes (format: `<route>.apps.<cluster_name>.<cluster_domain>`) |
| `cluster_toplevel_domain` | String | `example.com` | Yes | Domain for default user emails |

**Example:**
```yaml
cluster_name: "rhadp-prod"
cluster_domain: "openshift.mycompany.com"
cluster_toplevel_domain: "mycompany.com"
# Results in routes like: console-openshift-console.apps.rhadp-prod.openshift.mycompany.com
# And emails like: admin@mycompany.com
```

#### 5.1.5 Storage Configuration

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cluster_default_storage_class` | String | `gp3-csi` | Yes | Default storage class (AWS: `gp3-csi`, Azure: `managed-csi`, GCP: `standard-csi`) |

**Example:**
```yaml
cloud_provider: "azure"
cluster_default_storage_class: "managed-csi"
```

#### 5.1.6 Pull Secret

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `pull_secret_file` | String | `inventory/pull-secret.txt` | Yes | Path to Red Hat pull secret JSON file (obtain from https://console.redhat.com/openshift/create) |

**Example:**
```yaml
pull_secret_file: "$HOME/.secrets/openshift-pull-secret.json"
```

#### 5.1.7 Cluster Deployment Options

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `setup_arm_worker_nodes` | Boolean | `false` | No | Create additional ARM64 worker pool (hybrid workloads) |
| `setup_baremetal_worker_node` | Boolean | `false` | No | Create bare-metal ARM node (AWS m6g.metal only, for virtualization) |
| `setup_virtualization` | Boolean | `false` | No | Install OpenShift Virtualization (CNV) operator (requires `setup_baremetal_worker_node: true`) |

**Example:**
```yaml
setup_arm_worker_nodes: true
setup_baremetal_worker_node: true
setup_virtualization: true
```

### 5.2 Cloud Provider Credentials (Part 2)

#### 5.2.1 AWS Variables

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `aws_access_key_id` | String | - | Yes (AWS) | AWS IAM access key (20-char, starts with `AKIA`) - **Sensitive** |
| `aws_secret_access_key` | String | - | Yes (AWS) | AWS IAM secret key (40-char Base64) - **Sensitive, use Ansible Vault** |
| `aws_default_region` | String | - | Yes (AWS) | AWS region (e.g., `us-east-2`, `eu-west-1`) - verify instance type availability |

**Example:**
```yaml
aws_access_key_id: "AKIAIOSFODNN7EXAMPLE"
aws_secret_access_key: "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
aws_default_region: "us-east-2"
```

#### 5.2.2 Azure Variables

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `azure_guid` | String (UUID) | - | Yes (Azure) | Azure GUID identifier |
| `azure_client_id` | String (UUID) | - | Yes (Azure) | Service principal application/client ID - **Sensitive** |
| `azure_password` | String | - | Yes (Azure) | Service principal client secret - **Sensitive, use Ansible Vault** |
| `azure_tenant` | String | - | Yes (Azure) | Azure AD tenant ID (UUID) or domain (e.g., `company.onmicrosoft.com`) |
| `azure_subscription` | String (UUID) | - | Yes (Azure) | Azure subscription ID |
| `azure_resource_group` | String | - | Yes (Azure) | Resource group name (created if doesn't exist) |
| `azure_default_region` | String | - | Yes (Azure) | Azure region (e.g., `eastus`, `westeurope`) - verify ARM VM availability |

**Example:**
```yaml
azure_guid: "12345678-1234-1234-1234-123456789012"
azure_client_id: "87654321-4321-4321-4321-210987654321"
azure_password: "your-service-principal-password"
azure_tenant: "mycompany.onmicrosoft.com"
azure_subscription: "fedcba98-7654-3210-fedc-ba9876543210"
azure_resource_group: "rhadp-production-rg"
azure_default_region: "eastus"
```

#### 5.2.3 GCP Variables

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `gcp_service_account_key_file` | String (path) | - | Yes (GCP) | Path to service account JSON key file - **Sensitive** |
| `gcp_service_account_email` | String (email) | - | Yes (GCP) | Service account email (format: `name@project.iam.gserviceaccount.com`) |
| `gcp_project_id` | String | - | Yes (GCP) | GCP project ID (must have billing and APIs enabled) |
| `gcp_organization_id` | String | - | No | Numeric organization ID (for organization-level resources) |
| `gcp_default_region` | String | - | Yes (GCP) | GCP region (e.g., `us-central1`, `europe-west1`) - verify T2A availability |

**Example:**
```yaml
gcp_service_account_key_file: "$HOME/.gcp/openshift-sa.json"
gcp_service_account_email: "openshift-sa@my-project-123.iam.gserviceaccount.com"
gcp_project_id: "my-openshift-project"
gcp_organization_id: "123456789012"
gcp_default_region: "us-central1"
```

#### 5.2.4 GitHub Integration

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `github_endpoint` | String (URL) | `https://github.com` | No | GitHub API endpoint (use custom URL for GitHub Enterprise) |
| `github_org` | String | - | No | GitHub organization name for integration |
| `github_api_pat` | String | - | No | Personal Access Token - **Sensitive, use Ansible Vault** (scopes: `repo`, `admin:org`, `workflow`) |
| `github_git_client_id` | String | - | No | OAuth app client ID for Git operations - **Sensitive** |
| `github_git_client_secret` | String | - | No | OAuth app client secret for Git - **Sensitive, use Ansible Vault** |
| `github_oauth_client_id` | String | - | No | OAuth app client ID for Keycloak SSO - **Sensitive** (separate app from git client) |
| `github_oauth_client_secret` | String | - | No | OAuth app client secret for Keycloak - **Sensitive, use Ansible Vault** |

**Example:**
```yaml
github_endpoint: "https://github.com"
github_org: "my-automotive-company"
github_api_pat: "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
github_git_client_id: "Iv1.xxxxxxxxxxxxxxxx"
github_git_client_secret: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
github_oauth_client_id: "Iv1.yyyyyyyyyyyyyyyy"
github_oauth_client_secret: "yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy"
```

#### 5.2.5 Certificate Management

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `letsencrypt_email` | String (email) | `hello@example.com` | Yes | Email for Let's Encrypt cert notifications (use team/group email) |

**Example:**
```yaml
letsencrypt_email: "devops-team@mycompany.com"
```

### 5.3 Platform Deployment Options (Part 3)

#### 5.3.1 Feature Flags

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `rhadp_deploy_developer_hub` | Boolean | `false` | No | Deploy Red Hat Developer Hub (Backstage-based portal and service catalog) |
| `rhadp_deploy_jumpstart` | Boolean | `false` | No | Deploy Jumpstarter operator (hardware testing automation with QEMU support) |

**Example:**
```yaml
rhadp_deploy_developer_hub: true
rhadp_deploy_jumpstart: true
```

#### 5.3.2 Platform Configuration

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `rhadp_prefix` | String | `rhadp` | Yes | Prefix for platform namespaces (DNS-compatible: lowercase, alphanumeric, hyphens) |
| `rhadp_gitops_repo_url` | String (URL) | `https://github.com/rhadp/rhadp-manifests` | Yes | Git repository URL for ArgoCD application manifests |
| `rhadp_gitops_repo_revision` | String | `develop` | Yes | Git branch, tag, or commit SHA for ArgoCD sync |

**Example:**
```yaml
rhadp_prefix: "automotive"
rhadp_gitops_repo_url: "https://github.com/mycompany/automotive-platform-manifests"
rhadp_gitops_repo_revision: "main"  # or "v1.0.0" (tag) or "abc123def456" (commit)
```

#### 5.3.3 Platform Namespaces

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `platform_devspaces_namespace` | String | `{{ rhadp_prefix }}-devspaces` | Yes | Namespace for OpenShift Dev Spaces |
| `platform_developer_hub_namespace` | String | `{{ rhadp_prefix }}-devhub` | Yes | Namespace for Developer Hub |
| `platform_builder_namespace` | String | `{{ rhadp_prefix }}-builder` | Yes | Namespace for CI/CD builder infrastructure |
| `platform_jumpstarter_namespace` | String | `{{ rhadp_prefix }}-jumpstarter` | Yes | Namespace for Jumpstarter operator |

**Example:**
```yaml
# With rhadp_prefix="automotive", these templates result in:
platform_devspaces_namespace: "{{ rhadp_prefix }}-devspaces"      # automotive-devspaces
platform_developer_hub_namespace: "{{ rhadp_prefix }}-devhub"    # automotive-devhub
platform_builder_namespace: "{{ rhadp_prefix }}-builder"         # automotive-builder
platform_jumpstarter_namespace: "{{ rhadp_prefix }}-jumpstarter" # automotive-jumpstarter
```

### 5.4 Platform Internals (Part 4)

#### 5.4.1 Admin User Configuration

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `default_admin_user` | String | `admin` | Yes | Administrator username for htpasswd OAuth - **Change for production** |
| `default_admin_password` | String | `openshift` | Yes | Administrator password - **MUST CHANGE for production** (12+ chars, mixed case, numbers, symbols) |
| `default_admin_email` | String (email) | `admin@example.com` | Yes | Administrator email (used in Keycloak and notifications) |

**Example:**
```yaml
default_admin_user: "platform-admin"
default_admin_password: "SecureP@ssw0rd123!"
default_admin_email: "openshift-admin@mycompany.com"
```

#### 5.4.2 Test User Configuration

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `default_nobody_user` | String | `nobody` | No | Unprivileged test user account name |
| `default_nobody_password` | String | `nobody` | No | Unprivileged test user password |

**Example:**
```yaml
default_nobody_user: "developer"
default_nobody_password: "DevPassword123"
```

#### 5.4.3 Security Settings

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `remove_kubeadmin` | Boolean | `true` | No | Remove default kubeadmin user after deployment - **Recommended for production** |
| `remove_selfprovisioning` | Boolean | `true` | No | Remove self-provisioning role binding (prevents users creating own namespaces) |

**Example:**
```yaml
remove_kubeadmin: true
remove_selfprovisioning: true
```

#### 5.4.4 Keycloak Configuration

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `keycloak_db_user` | String | `admin` | Yes | PostgreSQL username for Keycloak - **Change for production** |
| `keycloak_db_password` | String | `openshift` | Yes | PostgreSQL password for Keycloak - **MUST CHANGE for production, use Ansible Vault** |
| `keycloak_client_secret` | String | `openshiftkc` | Yes | OAuth client secret for OpenShift-Keycloak integration - **MUST CHANGE** (32+ random chars) |

**Example:**
```yaml
keycloak_db_user: "keycloak_admin"
keycloak_db_password: "Keycl0akDB_P@ssw0rd!"
keycloak_client_secret: "randomly-generated-secret-string-here"
```

#### 5.4.5 Other Settings

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `remove_installer` | Boolean | `true` | No | Remove installer files after deployment (saves disk space, set `false` for debugging) |

**Example:**
```yaml
remove_installer: false  # Keep for debugging
```

---

## 6. Multi-Cloud Deployment

RHADP provides a unified deployment experience across AWS, Azure, and GCP. This section describes cloud-specific deployment characteristics.

### 6.1 AWS Deployment

#### 6.1.1 Infrastructure Overview

| Component | Configuration |
|-----------|--------------|
| **Deployment Mode** | Installer-provisioned infrastructure (IPI) |
| **Network** | Dedicated VPC with public/private subnets across 3 Availability Zones |
| **Load Balancers** | Network Load Balancers (NLB) for API (port 6443) and ingress (ports 80, 443) |
| **DNS** | Route 53 entries (if using AWS DNS) |
| **Image Registry** | S3 bucket backend storage |
| **Storage Class** | `gp3-csi` (General Purpose SSD v3) - EBS CSI driver |

#### 6.1.2 Instance Types

| Node Type | Instance Type | vCPU | Memory | Architecture | Count | Use Case |
|-----------|---------------|------|--------|--------------|-------|----------|
| **Control Plane** | `m8g.2xlarge` | 8 | 32 GiB | ARM64 (Graviton4) | 3 | Default control plane |
| **Worker Nodes** | `m8g.xlarge` | 4 | 16 GiB | ARM64 (Graviton4) | 3 | Default workers |
| **ARM Workers** (optional) | `m8g.xlarge` | 4 | 16 GiB | ARM64 (Graviton4) | Configurable | When `setup_arm_worker_nodes: true` |
| **Bare-Metal** (optional) | `m6g.metal` | 64 | 256 GiB | ARM64 (Graviton2) | 1 | Virtualization with nested virt |

**Override to x86:**
```yaml
aws_control_plane_instance_type: "m6a.2xlarge"  # x86_64
aws_control_plane_architecture: "amd64"
aws_worker_instance_type: "m6a.xlarge"          # x86_64
aws_worker_architecture: "amd64"
```

#### 6.1.3 Networking

| Component | Details |
|-----------|---------|
| **VPC** | Automatically created (customizable CIDR in role defaults) |
| **Availability Zones** | 3 zones for high availability |
| **Public Subnets** | Load balancers and NAT gateways |
| **Private Subnets** | Control plane and worker nodes |
| **Security Groups** | Automatic rules for OpenShift traffic (API, etcd, ingress, internal) |
| **Outbound Access** | NAT gateways for private subnets |
| **Load Balancing** | Cross-zone load balancing enabled |

#### 6.1.4 Storage Options

| EBS Volume Type | Description | Use Case |
|----------------|-------------|----------|
| **gp3** (default) | General Purpose SSD v3 | Cost-effective, good performance |
| **gp2** | General Purpose SSD (legacy) | Older generation |
| **io1, io2** | Provisioned IOPS SSD | High performance workloads |
| **st1** | Throughput Optimized HDD | Big data, data warehouses |
| **sc1** | Cold HDD | Infrequently accessed data |

**Configuration:** See `roles/config-cluster/defaults/main.yml` for volume type settings.

#### 6.1.5 AWS-Specific Features

| Feature | Benefits | Notes |
|---------|----------|-------|
| **Graviton ARM** | 20% cost savings, better performance-per-watt | Instance families: m6g, m7g, m8g, c6g, c7g, r6g, r7g |
| **Bare-Metal (m6g.metal)** | Nested virtualization, physical CPU features | Required for OpenShift Virtualization (CNV) |
| **Regional Availability** | Multiple regions worldwide | Verify Graviton availability in target region |

#### 6.1.6 Reference Documentation

- [Installing on AWS - OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/installing_on_aws/index)
- [AWS Regions and Availability Zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)
- [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
- [AWS Graviton Processors](https://aws.amazon.com/ec2/graviton/)

### 6.2 Azure Deployment

#### 6.2.1 Infrastructure Overview

| Component | Configuration |
|-----------|--------------|
| **Deployment Mode** | Installer-provisioned infrastructure (IPI) |
| **Network** | Virtual Network (VNet) with subnets across 3 Availability Zones |
| **Load Balancers** | Azure Load Balancers (Standard SKU) for API and ingress traffic |
| **DNS** | Azure DNS zones (if using Azure DNS) |
| **Image Registry** | Azure Blob Storage backend |
| **Storage Class** | `managed-csi` (Azure Managed Disks) - Azure Disk CSI driver |

#### 6.2.2 Instance Types

| Node Type | Instance Type | vCPU | Memory | Architecture | Count | Use Case |
|-----------|---------------|------|--------|--------------|-------|----------|
| **Control Plane** | `Standard_D8s_v5` | 8 | 32 GiB | x86_64 (AMD) | 3 | Default control plane |
| **Worker Nodes** | `Standard_D8s_v5` | 8 | 32 GiB | x86_64 (AMD) | 3 | Default workers |
| **ARM Workers** (optional) | `Standard_D8ps_v5` | 8 | 32 GiB | ARM64 (Ampere Altra) | Configurable | When `setup_arm_worker_nodes: true` |

**Override for larger instances:**
```yaml
azure_control_plane_instance_type: "Standard_D16s_v5"  # Larger control plane
azure_worker_instance_type: "Standard_D16s_v5"         # Larger workers
azure_arm_instance_type: "Standard_D16ps_v5"           # Larger ARM workers
```

#### 6.2.3 Networking

| Component | Details |
|-----------|---------|
| **VNet** | Automatically created (customizable address space in role defaults) |
| **Availability Zones** | 3 zones (where supported by region) |
| **Subnets** | Separate subnets for control plane and worker nodes |
| **Network Security Groups** | Automatic rules for OpenShift traffic (API, ingress, internal) |
| **Load Balancers** | Standard SKU for API and ingress with public IPs |
| **Outbound Connectivity** | Explicit outbound connectivity (Azure retiring default Sept 2025) |

#### 6.2.4 Storage Options

| Managed Disk Type | Description | Use Case |
|------------------|-------------|----------|
| **Premium_LRS** (default) | Premium SSD | High performance, low latency |
| **StandardSSD_LRS** | Standard SSD | Cost-effective, moderate performance |
| **Standard_LRS** | Standard HDD | Lowest cost, infrequent access |

**Configuration:** See `roles/create-cluster/defaults/main.yml` and `roles/config-cluster/defaults/main.yml`.

#### 6.2.5 Azure-Specific Features

| Feature | Benefits | Notes |
|---------|----------|-------|
| **Ampere Altra ARM (Dpsv5)** | Cost and performance benefits | Available in select regions |
| **Azure File Cross-Subscription** | Mount file shares from different subscription | Subscriptions must be in same tenant (OCP 4.19+) |
| **Instance Series** | Multiple optimized options | Dv5 (general x86), Dpsv5 (ARM), Dasv5 (AMD), Ev5 (memory) |

#### 6.2.6 Reference Documentation

- [Installing on Azure - OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_azure/index)
- [Azure Virtual Machine Sizes](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes)
- [Dv5 and Dsv5 Series](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/general-purpose/dv5-series)
- [Dpsv5 and Dplsv5 Series (ARM)](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/general-purpose/dpsv5-series)
- [Azure Regions](https://azure.microsoft.com/en-us/global-infrastructure/geographies/)

### 6.3 GCP Deployment

#### 6.3.1 Infrastructure Overview

| Component | Configuration |
|-----------|--------------|
| **Deployment Mode** | Installer-provisioned infrastructure (IPI) |
| **Network** | Custom VPC network with regional subnets across 3 Zones |
| **Load Balancers** | TCP LB for API (port 6443), HTTP(S) LB for ingress (ports 80, 443) |
| **DNS** | Cloud DNS managed zones (if using Cloud DNS) |
| **Image Registry** | Cloud Storage bucket backend |
| **Storage Class** | `standard-csi` (Standard Persistent Disk) - GCE Persistent Disk CSI driver |

#### 6.3.2 Instance Types

| Node Type | Instance Type | vCPU | Memory | Architecture | Count | Use Case |
|-----------|---------------|------|--------|--------------|-------|----------|
| **Control Plane** | `t2a-standard-8` | 8 | 32 GiB | ARM64 (Ampere Altra) | 3 | Default control plane |
| **Worker Nodes** | `t2a-standard-8` | 8 | 32 GiB | ARM64 (Ampere Altra) | 3 | Default workers |
| **ARM Workers** (optional) | `t2a-standard-8` | 8 | 32 GiB | ARM64 (Ampere Altra) | Configurable | When `setup_arm_worker_nodes: true` |

**Override to x86:**
```yaml
gcp_control_plane_instance_type: "n2-standard-8"  # x86_64
gcp_control_plane_architecture: "amd64"
gcp_worker_instance_type: "n2-standard-8"         # x86_64
gcp_worker_architecture: "amd64"
```

#### 6.3.3 Networking

| Component | Details |
|-----------|---------|
| **VPC** | Custom mode VPC created by installer |
| **Zones** | 3 zones within selected region for high availability |
| **Subnets** | Regional subnets (span all zones in region) |
| **Firewall Rules** | Automatic rules for OpenShift traffic (API, ingress, internal, health checks) |
| **Load Balancers** | TCP LB for API, HTTP(S) LB for ingress with global capabilities |
| **Cloud NAT** | Outbound internet access for private instances (no public IPs needed) |

#### 6.3.4 Storage Options

| Persistent Disk Type | Description | Use Case |
|---------------------|-------------|----------|
| **pd-standard** | Standard persistent disks (HDD) | Cost-effective |
| **pd-balanced** | Balanced persistent disks (SSD) | Good balance of performance and cost |
| **pd-ssd** | SSD persistent disks | High performance |
| **pd-extreme** | Extreme persistent disks | Highest performance, customizable IOPS |

**Configuration:** See `roles/config-cluster/defaults/main.yml` for volume type settings.

#### 6.3.5 GCP-Specific Features

| Feature | Benefits | Notes |
|---------|----------|-------|
| **T2A Ampere Altra ARM** | 30% price-performance improvement vs x86 | T2A instance family (t2a-standard-1 to t2a-standard-48) |
| **Confidential Computing** | AMD SEV, Intel TDX support | Encrypted memory VMs (N2D, C2D instances, OCP 4.19+) |
| **Machine Type Families** | Multiple optimized options | T2A (ARM), N2 (Intel), N2D (AMD), C2 (compute), E2 (cost) |

#### 6.3.6 Reference Documentation

- [Installing on GCP - OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_gcp/index)
- [Google Compute Engine Machine Types](https://cloud.google.com/compute/docs/machine-types)
- [Tau T2A Machine Series (ARM)](https://cloud.google.com/compute/docs/general-purpose-machines#t2a_machines)
- [GCP Regions and Zones](https://cloud.google.com/compute/docs/regions-zones)
- [Confidential Computing](https://cloud.google.com/confidential-computing)

---

## 7. Multi-Architecture Support

RHADP provides comprehensive support for multiple CPU architectures, enabling flexible deployment options for diverse workload requirements.

### 7.1 x86_64 Clusters

#### 7.1.1 Overview
- **Architecture**: AMD64/x86_64 processors (Intel, AMD)
- **Use Case**: Standard enterprise workloads, maximum software compatibility
- **Configuration**: Set `cluster_arch: "x86_64"` in inventory

#### 7.1.2 Characteristics
- Broadest ecosystem support for container images
- Highest compatibility with third-party software
- Mature tooling and debugging support
- Widely available across all cloud providers

#### 7.1.3 Instance Types

**AWS:**
- Control Plane: `m6a.2xlarge` (AMD EPYC)
- Workers: `m6a.xlarge` (AMD EPYC)

**Azure:**
- Control Plane: `Standard_D8s_v5` (Intel/AMD)
- Workers: `Standard_D8s_v5` (Intel/AMD)

**GCP:**
- Control Plane: `n2-standard-8` (Intel Cascade Lake/Ice Lake)
- Workers: `n2-standard-8` (Intel Cascade Lake/Ice Lake)

#### 7.1.4 Configuration Example
```yaml
cluster_arch: "x86_64"
aws_control_plane_architecture: "amd64"
aws_worker_architecture: "amd64"
```

### 7.2 ARM64 (aarch64) Clusters

#### 7.2.1 Overview
- **Architecture**: ARM64/aarch64 processors (Graviton, Ampere Altra)
- **Use Case**: Cost optimization, power efficiency, automotive embedded systems
- **Configuration**: Set `cluster_arch: "aarch64"` in inventory

#### 7.2.2 Characteristics
- **Cost Savings**: 10-20% lower cost vs x86_64 instances
- **Power Efficiency**: Better performance-per-watt for many workloads
- **Automotive Alignment**: ARM architecture common in automotive ECUs
- **Growing Ecosystem**: Increasing container image availability for ARM64

#### 7.2.3 Instance Types

**AWS (Graviton2/3/4):**
- Control Plane: `m8g.2xlarge` (Graviton4)
- Workers: `m8g.xlarge` (Graviton4)
- Bare-Metal: `m6g.metal` (Graviton2, for virtualization)

**Azure (Ampere Altra):**
- Control Plane: Can use x86 (Dv5) with ARM workers
- Workers: `Standard_D8ps_v5` (Ampere Altra ARM)

**GCP (Ampere Altra):**
- Control Plane: `t2a-standard-8` (Ampere Altra)
- Workers: `t2a-standard-8` (Ampere Altra)

#### 7.2.4 Configuration Example
```yaml
cluster_arch: "aarch64"
aws_control_plane_architecture: "arm64"
aws_worker_architecture: "arm64"
```

#### 7.2.5 Considerations
- Verify container images support ARM64 architecture
- Some legacy or proprietary software may not have ARM builds
- Cross-compilation may be required for custom applications
- Performance characteristics differ from x86_64 (benchmark workloads)

### 7.3 Hybrid (Multi-Architecture) Clusters

#### 7.3.1 Overview
- **Architecture**: Mixed x86_64 and ARM64 nodes in same cluster
- **Use Case**: Gradual migration, workload-specific optimization, development flexibility
- **Configuration**: Set `cluster_arch: "multi"` in inventory
- **Platform Support**: **AWS and Azure only** (GCP not supported for multi-arch)

#### 7.3.2 Characteristics
- Run x86 and ARM workloads in same cluster
- Kubernetes scheduler automatically places pods on compatible nodes
- Flexible resource allocation based on workload requirements
- Test ARM compatibility while maintaining x86 fallback

#### 7.3.3 Architecture Patterns

**Pattern 1: x86 Control Plane + ARM Workers**
```yaml
cluster_arch: "multi"
control_plane_arch: "amd64"  # x86_64 control plane
setup_arm_worker_nodes: true  # Add ARM worker pool
```

**Pattern 2: ARM Control Plane + x86 Workers**
```yaml
cluster_arch: "multi"
control_plane_arch: "arm64"  # ARM control plane
# Default workers are x86, ARM workers added via setup_arm_worker_nodes
```

**Pattern 3: Mixed Worker Pools**
```yaml
cluster_arch: "x86_64"  # Primary architecture
setup_arm_worker_nodes: true  # Add secondary ARM pool
# Results in both x86 and ARM workers
```

#### 7.3.4 Workload Scheduling

**Node Selectors:**
Kubernetes automatically adds architecture labels to nodes:
```yaml
kubernetes.io/arch: amd64  # x86_64 nodes
kubernetes.io/arch: arm64  # ARM nodes
```

**Pod Scheduling Examples:**
```yaml
# Force pod to run on ARM nodes
spec:
  nodeSelector:
    kubernetes.io/arch: arm64

# Force pod to run on x86 nodes
spec:
  nodeSelector:
    kubernetes.io/arch: amd64

# Let scheduler choose based on availability (multi-arch image required)
spec:
  # No nodeSelector - pod can run on any compatible node
```

**Multi-Architecture Container Images:**
- Use manifest lists to provide both x86 and ARM images
- Docker/Podman automatically pull correct architecture
- Example: `quay.io/image:tag` (manifest list with amd64 and arm64)

#### 7.3.5 Configuration Example

**AWS Multi-Arch Cluster:**
```yaml
cloud_provider: "aws"
cluster_arch: "multi"
control_plane_arch: "amd64"  # x86 control plane

# Primary workers (set architecture in role defaults)
aws_worker_architecture: "amd64"  # x86 workers

# Secondary ARM workers
setup_arm_worker_nodes: true
aws_arm_instance_type: "m8g.xlarge"
aws_arm_instance_count: 3
```

**Azure Multi-Arch Cluster:**
```yaml
cloud_provider: "azure"
cluster_arch: "multi"
control_plane_arch: "amd64"

azure_worker_architecture: "amd64"
setup_arm_worker_nodes: true
azure_arm_instance_type: "Standard_D8ps_v5"
azure_arm_instance_count: 3
```

#### 7.3.6 Use Cases

**Gradual Migration:**
- Start with x86 cluster
- Add ARM worker nodes
- Migrate workloads incrementally
- Validate ARM compatibility before full migration

**Cost Optimization:**
- Run cost-sensitive workloads on ARM (lower instance costs)
- Run legacy workloads on x86 (compatibility)
- Optimize total cost of ownership

**Development and Testing:**
- Develop on x86 (familiar tooling)
- Test on ARM (production target)
- Single cluster for entire workflow

**Automotive Workloads:**
- Build on x86 (fast compilation)
- Test on ARM (target ECU architecture)
- CI/CD in single cluster

#### 7.3.7 Considerations

**Container Image Compatibility:**
- Ensure all container images support required architectures
- Use manifest lists for multi-arch images
- Build separate images for x86 and ARM if needed

**Performance Differences:**
- Benchmark workloads on both architectures
- ARM may be faster/slower depending on workload
- Memory and I/O performance characteristics differ

**Instance Availability:**
- Verify both x86 and ARM instances available in target region
- Not all instance types available in all availability zones
- Plan capacity across both architecture pools

**Cost Analysis:**
- Calculate blended cost across instance types
- ARM instances typically 10-20% cheaper
- Balance cost savings vs performance requirements

---

## 8. Platform Services

### 8.1 OpenShift Dev Spaces

#### 8.1.1 Overview
OpenShift Dev Spaces provides cloud-based developer workspaces accessible through a web browser, eliminating the need for local development environment setup.

**Key Features:**
- Browser-based IDE (based on Eclipse Che)
- Pre-configured development environments
- Consistent tooling across team
- Integrated Git operations
- Terminal access and debugging

#### 8.1.2 Accessing Dev Spaces

**URL Format:**
```
https://devspaces.apps.<cluster_name>.<cluster_domain>
```

**Example:**
```
https://devspaces.apps.rhadp.rhadp.example.com
```

**Login:**
- Use OpenShift credentials (admin user or Keycloak SSO)
- OAuth redirect from Dev Spaces to OpenShift

#### 8.1.3 Creating Workspaces

**From Git Repository:**
1. Click "Create Workspace"
2. Enter Git repository URL
3. Dev Spaces analyzes repo for devfile
4. Workspace launches with appropriate tools

**From Devfile:**
- Create `devfile.yaml` in repository root
- Define container images, tools, and commands
- Dev Spaces provisions workspace based on devfile

**Example Devfile:**
```yaml
schemaVersion: 2.2.0
metadata:
  name: automotive-workspace
components:
  - name: dev-tools
    container:
      image: quay.io/devfile/universal-developer-image:latest
      memoryLimit: 2Gi
      endpoints:
        - name: web-app
          targetPort: 8080
commands:
  - id: build
    exec:
      component: dev-tools
      commandLine: make build
```

#### 8.1.4 Using the Browser IDE

**Features:**
- Monaco editor (VS Code editor component)
- Syntax highlighting for multiple languages
- IntelliSense and code completion
- Integrated terminal
- Git operations (commit, push, pull)
- Debugger integration

**Workflow:**
1. Open workspace from Dev Spaces dashboard
2. Edit code in browser
3. Run build/test commands in terminal
4. Commit and push changes via Git panel
5. Stop workspace when done (resources released)

#### 8.1.5 Workspace Configuration

**Resource Limits:**
- CPU and memory limits defined in devfile
- Default limits configurable by admin
- Workspaces stopped after inactivity period

**Persistent Storage:**
- Workspace files persisted in PVCs
- Projects persist across workspace restarts
- Configure storage size in devfile or admin settings

### 8.2 Developer Hub (Optional)

Developer Hub provides a unified portal for developers to discover services, APIs, and platform capabilities.

**Access URL:**
```
https://developer-hub-<namespace>.apps.<cluster_name>.<cluster_domain>
```

**Deployment:**
Set `rhadp_deploy_developer_hub: true` in inventory to enable.

**Features:**
- Service catalog
- API documentation
- Project templates
- Team collaboration
- Plugin ecosystem

### 8.3 OpenShift GitOps

#### 8.3.1 Overview
OpenShift GitOps (based on ArgoCD) provides declarative, Git-based infrastructure and application management.

**Key Concepts:**
- **Application**: Kubernetes resources managed as a unit
- **Repository**: Git repo containing manifests
- **Sync**: Process of applying Git state to cluster
- **Sync Policy**: Automatic or manual sync

#### 8.3.2 Accessing ArgoCD UI

**URL:**
```
https://argocd-server-openshift-gitops.apps.<cluster_name>.<cluster_domain>
```

**Login:**
- Use OpenShift credentials
- OAuth integration with OpenShift

#### 8.3.3 GitOps Workflows

**Application Deployment Pattern:**
1. Define Kubernetes manifests in Git repository
2. Create ArgoCD Application pointing to repo
3. ArgoCD monitors repo for changes
4. On commit, ArgoCD syncs changes to cluster
5. Applications stay in sync with Git state

**Application Example:**
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: automotive-app
  namespace: openshift-gitops
spec:
  project: default
  source:
    repoURL: https://github.com/myorg/automotive-app
    targetRevision: main
    path: k8s-manifests
  destination:
    server: https://kubernetes.default.svc
    namespace: automotive-prod
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

#### 8.3.4 Repository Structure

**Recommended Layout:**
```
rhadp-manifests/
├── applications/          # ArgoCD Application definitions
│   ├── dev-spaces.yaml
│   ├── jumpstarter.yaml
│   └── custom-app.yaml
├── dev-spaces/            # Dev Spaces manifests
│   ├── namespace.yaml
│   └── che-cluster.yaml
├── jumpstarter/           # Jumpstarter manifests
│   └── operator.yaml
└── custom-app/            # Custom application
    ├── deployment.yaml
    ├── service.yaml
    └── route.yaml
```

**Configuration:**
Set `rhadp_gitops_repo_url` and `rhadp_gitops_repo_revision` in inventory.

### 8.4 OpenShift Pipelines

#### 8.4.1 Overview
OpenShift Pipelines (based on Tekton) provides cloud-native CI/CD capabilities for building, testing, and deploying applications.

**Key Concepts:**
- **Task**: Reusable step in pipeline (e.g., build, test, deploy)
- **Pipeline**: Sequence of Tasks
- **PipelineRun**: Execution instance of Pipeline
- **Trigger**: Automatic pipeline execution on events (Git push, webhook)

#### 8.4.2 Creating Tekton Pipelines

**Example Pipeline:**
```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: build-and-deploy
spec:
  params:
    - name: git-url
      type: string
    - name: git-revision
      type: string
      default: main
  workspaces:
    - name: source
  tasks:
    - name: fetch-source
      taskRef:
        name: git-clone
      params:
        - name: url
          value: $(params.git-url)
        - name: revision
          value: $(params.git-revision)
      workspaces:
        - name: output
          workspace: source
    - name: build-image
      taskRef:
        name: buildah
      runAfter:
        - fetch-source
      workspaces:
        - name: source
          workspace: source
    - name: deploy
      taskRef:
        name: openshift-client
      runAfter:
        - build-image
```

#### 8.4.3 Pipeline Runs and Triggers

**Manual Pipeline Run:**
```bash
tkn pipeline start build-and-deploy \
  --param git-url=https://github.com/myorg/app \
  --workspace name=source,claimName=pipeline-pvc \
  --showlog
```

**Git Webhook Trigger:**
Configure EventListener to trigger pipeline on Git push events.

#### 8.4.4 Integration with Git Repositories

**GitHub Integration:**
- Configure webhook in GitHub repository
- Point to EventListener route in OpenShift
- Pipeline runs automatically on push

**GitLab Integration:**
- Similar webhook configuration
- Use GitLab-specific EventListener

### 8.5 Jumpstarter (Optional)

#### 8.5.1 Overview
Jumpstarter provides automated testing on real and virtual hardware, ideal for automotive ECU and embedded system testing.

**Deployment:**
Set `rhadp_deploy_jumpstart: true` in inventory to enable.

#### 8.5.2 Hardware Testing Automation

**Features:**
- Remote device management
- QEMU exporter for virtual hardware
- CI/CD integration for automated testing
- Support for real hardware (via exporters)

#### 8.5.3 QEMU Exporter Usage

**Virtual Device Testing:**
- Spawn QEMU VMs for testing
- Emulate ARM or x86 hardware
- Integrate with CI/CD pipelines

#### 8.5.4 CI/CD Integration for Testing

**Pipeline Integration:**
1. Build software in OpenShift Pipelines
2. Trigger Jumpstarter test on virtual/real hardware
3. Collect test results
4. Report pass/fail status
5. Deploy on success

**Use Case: Automotive ECU Testing:**
- Build ECU software in pipeline
- Flash to virtual ECU (QEMU)
- Run automated test suite
- Validate before deploying to physical ECUs

---

## 9. Cluster Management

### 9.1 Lifecycle Operations

#### 9.1.1 Starting a Cluster

**Command:**
```bash
ansible-playbook -i inventory/ start.yml
```

**What Happens:**
- Starts all stopped cloud instances (control plane and worker nodes)
- Waits for instances to become running
- Waits for Kubernetes API to become available
- Verifies cluster operators are healthy

**Use Case:**
- Resume work after stopping cluster to save costs
- Start cluster on demand for development/testing

**Cloud Provider Actions:**
- **AWS**: Starts EC2 instances via `aws ec2 start-instances`
- **Azure**: Starts VMs via `az vm start`
- **GCP**: Starts instances via `gcloud compute instances start`

**Startup Time:**
- Typically 5-10 minutes for cluster to become fully operational
- Depends on number of nodes and cloud provider responsiveness

#### 9.1.2 Stopping a Cluster

**Command:**
```bash
ansible-playbook -i inventory/ stop.yml
```

**What Happens:**
- Gracefully stops all worker nodes
- Stops control plane nodes
- Instances remain in stopped state (not terminated)
- Persistent volumes and data retained

**Use Case:**
- Save cloud costs during nights/weekends
- Pause development clusters when not in use

**Cloud Provider Actions:**
- **AWS**: Stops EC2 instances via `aws ec2 stop-instances`
- **Azure**: Deallocates VMs via `az vm deallocate`
- **GCP**: Stops instances via `gcloud compute instances stop`

**Cost Savings:**
- No compute charges while stopped
- Still charged for storage (EBS volumes, persistent disks)
- Load balancers may still incur charges (cloud provider dependent)

#### 9.1.3 Cost Optimization with Stop/Start

**Best Practices:**
- Stop non-production clusters outside business hours
- Use automation (cron jobs) to start/stop on schedule
- Monitor costs with cloud provider billing tools
- Consider destroying rather than stopping for long-term idle periods

**Example Cost Savings (AWS m8g.xlarge worker):**
- Running 24/7: ~$350/month (3 workers)
- Running 8 hours/day, 5 days/week: ~$100/month (3 workers)
- **Savings**: ~70% reduction

### 9.2 Cluster Reconfiguration

#### 9.2.1 Overview
Reconfigure existing cluster to add features, change topology, or update settings without full redeployment.

**Command:**
```bash
ansible-playbook -i inventory/ 2_config_cluster.yml
```

#### 9.2.2 Adding Worker Nodes

**Add ARM Worker Nodes:**
1. Edit `inventory/main.yml`:
   ```yaml
   setup_arm_worker_nodes: true
   ```
2. Run reconfiguration playbook:
   ```bash
   ansible-playbook -i inventory/ 2_config_cluster.yml
   ```
3. Verify new nodes:
   ```bash
   oc get nodes -l kubernetes.io/arch=arm64
   ```

**Add Bare-Metal Nodes:**
1. Edit `inventory/main.yml`:
   ```yaml
   setup_baremetal_worker_node: true
   ```
2. Run reconfiguration playbook
3. Wait for bare-metal node provisioning (15-20 minutes)

#### 9.2.3 Modifying Cluster Settings

**Reconfiguration Scenarios:**
- Add or remove worker node pools
- Install OpenShift Virtualization
- Update cert-manager configuration
- Reconfigure authentication providers
- Update GitOps repository settings

**Playbook Behavior:**
- Idempotent: Safe to run multiple times
- Only applies changes (doesn't recreate existing resources)
- Can be run on live cluster

### 9.3 Cluster Destruction

#### 9.3.1 Destroying the Cluster

**Command:**
```bash
ansible-playbook -i inventory/ 9_destroy_cluster.yml
```

**What Happens:**
- Runs OpenShift installer destroy command
- Deletes all cloud resources:
  - EC2 instances / VMs
  - VPCs / VNets
  - Load balancers
  - Security groups / NSGs
  - Persistent volumes
  - DNS records
- Removes local cluster metadata

**Duration:**
- Typically 10-20 minutes
- Depends on number of resources and cloud provider API responsiveness

#### 9.3.2 Cleanup Verification

**Verify Resource Deletion:**

**AWS:**
```bash
aws ec2 describe-instances --filters "Name=tag:Name,Values=*rhadp*"
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=*rhadp*"
aws elb describe-load-balancers | grep rhadp
```

**Azure:**
```bash
az group show --name rhadp-rg  # Should return error if deleted
az vm list --resource-group rhadp-rg
```

**GCP:**
```bash
gcloud compute instances list --filter="name~rhadp"
gcloud compute networks list --filter="name~rhadp"
```

#### 9.3.3 Manual Cleanup (If Needed)

**Stuck Resources:**
Sometimes cloud resources fail to delete automatically. Manual cleanup may be required.

**AWS:**
```bash
# Delete VPC and dependencies
aws ec2 delete-vpc --vpc-id vpc-xxxxx

# Delete security groups
aws ec2 delete-security-group --group-id sg-xxxxx

# Delete load balancers
aws elb delete-load-balancer --load-balancer-name rhadp-xxx
```

**Azure:**
```bash
# Delete resource group (deletes all resources)
az group delete --name rhadp-rg --yes --no-wait
```

**GCP:**
```bash
# Delete network
gcloud compute networks delete rhadp-network

# Delete instances
gcloud compute instances delete instance-name --zone us-central1-a
```

#### 9.3.4 Local Cleanup

**Remove Cluster Metadata:**
```bash
rm -rf $HOME/.openshift/<cluster-name>-<cloud-provider>
rm -f $HOME/.ssh/<cluster-name>-key
rm -f $HOME/.ssh/<cluster-name>-key.pub
```

**Remove Installer Binary (Optional):**
```bash
rm bin/openshift-install
```

---

## 10. Security Considerations

### 10.1 Credential Management

#### 10.1.1 Storing Credentials Securely

**Inventory File Encryption:**
Use Ansible Vault to encrypt sensitive data in `inventory/main.yml`:

```bash
# Encrypt entire inventory file
ansible-vault encrypt inventory/main.yml

# Edit encrypted file
ansible-vault edit inventory/main.yml

# Run playbook with encrypted inventory
ansible-playbook -i inventory/ 0_all.yml --ask-vault-pass
```

**Encrypt Specific Variables:**
```bash
# Encrypt a variable value
ansible-vault encrypt_string 'secret-password' --name 'aws_secret_access_key'
```

**Vault Password File:**
```bash
# Store vault password in file (secure with file permissions)
echo 'my-vault-password' > .vault_pass
chmod 600 .vault_pass

# Run playbook with vault password file
ansible-playbook -i inventory/ 0_all.yml --vault-password-file .vault_pass
```

#### 10.1.2 Using Environment Variables

**Export Cloud Credentials:**
```bash
# AWS
export AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
export AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
export AWS_DEFAULT_REGION="us-east-2"

# Azure
export AZURE_CLIENT_ID="..."
export AZURE_SECRET="..."
export AZURE_TENANT="..."
export AZURE_SUBSCRIPTION_ID="..."

# GCP
export GOOGLE_APPLICATION_CREDENTIALS="$HOME/.gcp/osServiceAccount.json"
export GCP_PROJECT="my-project"
```

**Reference in Inventory:**
```yaml
aws_access_key_id: "{{ lookup('env', 'AWS_ACCESS_KEY_ID') }}"
aws_secret_access_key: "{{ lookup('env', 'AWS_SECRET_ACCESS_KEY') }}"
```

#### 10.1.3 Ansible Vault for Sensitive Data

**Best Practices:**
- Encrypt all passwords, API tokens, and secrets
- Use separate vault files for different environments (dev, prod)
- Store vault password securely (password manager, not in Git)
- Never commit unencrypted credentials to version control

**Vault File Structure:**
```
inventory/
├── main.yml                # Non-sensitive variables
├── vault.yml               # Encrypted sensitive variables
└── .gitignore              # Ignore vault password file
```

#### 10.1.4 Rotating Credentials

**Regular Rotation Schedule:**
- Cloud provider credentials: Every 90 days
- Admin passwords: After each use or every 30 days
- OAuth client secrets: Every 180 days
- Keycloak secrets: Every 90 days

**Rotation Process:**
1. Generate new credentials in cloud provider console
2. Update `inventory/main.yml` with new credentials
3. Re-run relevant playbooks to apply changes
4. Verify cluster functionality
5. Revoke old credentials

### 10.2 Network Security

#### 10.2.1 Firewall Rules and Security Groups

**Cloud Provider Defaults:**
- RHADP creates minimal security groups/firewall rules
- Only required OpenShift traffic allowed
- Principle of least privilege

**Review Security Groups (AWS):**
```bash
aws ec2 describe-security-groups --filters "Name=tag:Name,Values=*rhadp*"
```

**Common Rules:**
- API server (6443): Restricted to admin IP ranges
- HTTP/HTTPS (80/443): Public for ingress/router
- SSH (22): Restricted to bastion or admin IPs
- Internal: Node-to-node communication

#### 10.2.2 Ingress/Egress Configuration

**Ingress:**
- Default: Public ingress for cluster routes
- Restrict: Use load balancer source IP allowlists
- Internal: Deploy internal load balancers for private access

**Egress:**
- Default: Unrestricted outbound internet access
- Lockdown: Use network policies to restrict pod egress
- Proxy: Route egress through proxy servers

#### 10.2.3 Private vs Public Clusters

**Public Cluster (Default):**
- API server accessible from internet
- Ingress routes publicly accessible
- Worker nodes in private subnets

**Private Cluster:**
- API server accessible only from VPC/VNet
- Ingress accessible via VPN or Direct Connect
- Requires manual configuration (not default in RHADP)

### 10.3 Certificate Management

#### 10.3.1 Let's Encrypt Certificates

**Automatic Issuance:**
- cert-manager operator handles certificate lifecycle
- Let's Encrypt ClusterIssuer configured during deployment
- Certificates automatically issued for ingress routes

**Verification:**
```bash
# Check certificates
oc get certificate -A

# Check ClusterIssuer
oc get clusterissuer

# Verify cert-manager operator
oc get pods -n cert-manager
```

#### 10.3.2 Certificate Renewal

**Automatic Renewal:**
- cert-manager automatically renews certificates before expiration
- Default: Renew 30 days before expiry
- No manual intervention required

**Monitor Renewal:**
```bash
# Check certificate status
oc describe certificate <cert-name> -n <namespace>

# Check for renewal events
oc get events -n <namespace> | grep -i certificate
```

#### 10.3.3 Custom CA Certificates

**Use Custom CA:**
If Let's Encrypt is not suitable (internal clusters, policy requirements):

1. Create CA certificate and key
2. Create cert-manager Issuer with custom CA
3. Update route annotations to use custom issuer

**Example Custom Issuer:**
```yaml
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: internal-ca-issuer
  namespace: default
spec:
  ca:
    secretName: internal-ca-secret
```

### 10.4 Access Control

#### 10.4.1 RBAC Configuration

**Default RBAC:**
- Admin user: cluster-admin role
- Regular users: No default permissions (if `remove_selfprovisioning: true`)
- Service accounts: Namespace-specific permissions

**Grant User Access:**
```bash
# Create project
oc new-project automotive-dev

# Grant developer access
oc adm policy add-role-to-user edit developer-user -n automotive-dev

# Grant view access
oc adm policy add-role-to-user view readonly-user -n automotive-dev

# Grant cluster admin
oc adm policy add-cluster-role-to-user cluster-admin admin-user
```

#### 10.4.2 User and Group Management

**Create Users (htpasswd):**
```bash
# Generate password hash
htpasswd -nb newuser password123

# Update htpasswd secret
oc extract secret/htpasswd-secret -n openshift-config --to=- > users.htpasswd
htpasswd -b users.htpasswd newuser password123
oc set data secret/htpasswd-secret -n openshift-config --from-file=htpasswd=users.htpasswd
```

**Create Groups:**
```bash
# Create group
oc adm groups new automotive-team

# Add users to group
oc adm groups add-users automotive-team user1 user2

# Grant group access
oc adm policy add-role-to-group edit automotive-team -n automotive-dev
```

#### 10.4.3 OAuth Provider Integration

**Keycloak Integration:**
- Keycloak deployed and configured automatically
- OAuth provider for OpenShift authentication
- Supports external identity providers (GitHub, LDAP, SAML)

**GitHub OAuth (via Keycloak):**
1. Configure `github_oauth_client_id` and `github_oauth_client_secret` in inventory
2. Re-run authentication setup:
   ```bash
   ansible-playbook -i inventory/ 2_config_cluster.yml --tags auth
   ```
3. Users can log in with GitHub accounts

**Verify OAuth Providers:**
```bash
oc get oauth cluster -o yaml
```

---

## 11. Reference

### 11.1 Official OpenShift Documentation

**OpenShift Container Platform 4.19 Documentation:**
- [OCP 4.19 Documentation Portal](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19)
- [Installing on AWS](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/installing_on_aws/index)
- [Installing on Azure](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_azure/index)
- [Installing on GCP](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_gcp/index)
- [Release Notes](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/release_notes/index)

**Installation Guides:**
- [Preparing to install on AWS](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/installing_on_aws/preparing-to-install-on-aws)
- [Preparing to install on Azure](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_azure/index#preparing-to-install-on-azure)
- [Preparing to install on GCP](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_gcp/index#preparing-to-install-on-gcp)

**Platform Services:**
- [OpenShift GitOps Documentation](https://docs.openshift.com/gitops/latest/)
- [OpenShift Pipelines Documentation](https://docs.openshift.com/pipelines/latest/)
- [OpenShift Dev Spaces Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/)
- [OpenShift Virtualization Documentation](https://docs.openshift.com/container-platform/4.19/virt/about_virt/about-virt.html)

### 11.2 Related Projects

**RHADP Repositories:**
- [RHADP Platform Repository](https://github.com/rhadp/rhadp) - Infrastructure as Code for platform deployment
- [RHADP Manifests Repository](https://github.com/rhadp/rhadp-manifests) - GitOps application manifests

**Related Technologies:**
- [Jumpstarter Project](https://github.com/jumpstarter-dev) - Hardware testing automation framework
- [Ansible Documentation](https://docs.ansible.com/) - Automation engine used by RHADP
- [Tekton Documentation](https://tekton.dev/docs/) - Cloud-native CI/CD framework

**Red Hat Resources:**
- [Red Hat OpenShift Cluster Manager](https://console.redhat.com/openshift) - Cluster management portal
- [Red Hat Container Catalog](https://catalog.redhat.com/software/containers/explore) - Certified container images
- [Red Hat Developer](https://developers.redhat.com/) - Developer resources and tutorials

### 11.3 Ansible Roles Documentation

**Core Roles:**
- `create-cluster`: [roles/create-cluster/README.md](../roles/create-cluster/README.md) - Cluster bootstrapping
- `config-cluster`: [roles/config-cluster/README.md](../roles/config-cluster/README.md) - Topology configuration
- `gather-cluster-facts`: [roles/gather-cluster-facts/README.md](../roles/gather-cluster-facts/README.md) - Cluster discovery
- `install-operator`: [roles/install-operator/README.md](../roles/install-operator/README.md) - Operator installation

**Platform Roles:**
- `setup-auth`: Authentication and Keycloak setup
- `setup-cert-manager`: Certificate management configuration
- `setup-devops`: GitOps and Pipelines installation
- `setup-platform`: Dev Spaces and Developer Hub deployment
- `setup-jumpstarter`: Jumpstarter operator deployment

**Lifecycle Roles:**
- `lifecycle-cluster`: Cluster start/stop operations
- `create-builder`: Builder VM creation
- `setup-runner`: GitHub/GitLab runner configuration

### 11.4 Community and Support

**Project Resources:**
- **GitHub Issues**: [https://github.com/rhadp/rhadp/issues](https://github.com/rhadp/rhadp/issues) - Report bugs and request features
- **Project Board**: [https://github.com/orgs/rhadp/projects/1](https://github.com/orgs/rhadp/projects/1) - Track development progress
- **Contributing**: See [CONTRIBUTING.md](../CONTRIBUTING.md) for contribution guidelines

**Red Hat Support:**
- **Red Hat Customer Portal**: [https://access.redhat.com](https://access.redhat.com) - For Red Hat subscription customers
- **Red Hat Knowledgebase**: Technical articles and solutions
- **Red Hat Support Cases**: Open support tickets for subscribed products

**Community Forums:**
- **OpenShift Commons**: Community discussions and collaboration
- **Kubernetes Slack**: #openshift channel
- **Red Hat Developer Forums**: Developer community support

**Disclaimer:**
This is not an officially supported Red Hat product. For production use, consult Red Hat support and follow enterprise deployment best practices.

---

## Appendices

### Appendix A: Example Configurations

#### A.1 Minimal Development Cluster (x86_64, AWS)

**Use Case**: Small development cluster for testing and experimentation

**Configuration** (`inventory/main.yml`):
```yaml
cloud_provider: "aws"
cluster_name: "dev"
cluster_domain: "dev.example.com"
cluster_version: "4.19.14"
cluster_arch: "x86_64"
cluster_topology: "compact"  # Control plane nodes also run workloads

installer_architecture: "linux"

aws_access_key_id: "{{ lookup('env', 'AWS_ACCESS_KEY_ID') }}"
aws_secret_access_key: "{{ lookup('env', 'AWS_SECRET_ACCESS_KEY') }}"
aws_default_region: "us-east-2"

# Disable optional features
setup_arm_worker_nodes: false
setup_baremetal_worker_node: false
setup_virtualization: false
rhadp_deploy_developer_hub: false
rhadp_deploy_jumpstart: false

letsencrypt_email: "devops@example.com"
```

**Cost**: ~$400-500/month (3 control plane nodes in compact mode)

#### A.2 Production Cluster (Multi-Arch, Azure)

**Use Case**: Production automotive development platform with hybrid architecture

**Configuration** (`inventory/main.yml`):
```yaml
cloud_provider: "azure"
cluster_name: "rhadp-prod"
cluster_domain: "rhadp.mycompany.com"
cluster_version: "4.19.14"
cluster_arch: "multi"
control_plane_arch: "amd64"
cluster_topology: "default"

installer_architecture: "linux"

# Azure credentials (use Ansible Vault)
azure_client_id: "{{ vault_azure_client_id }}"
azure_password: "{{ vault_azure_password }}"
azure_tenant: "mycompany.onmicrosoft.com"
azure_subscription: "{{ vault_azure_subscription }}"
azure_resource_group: "rhadp-production-rg"
azure_default_region: "eastus"

# Multi-arch configuration
setup_arm_worker_nodes: true  # Add ARM worker pool

# Enable all platform features
rhadp_deploy_developer_hub: true
rhadp_deploy_jumpstart: true

# GitHub integration
github_org: "mycompany"
github_api_pat: "{{ vault_github_pat }}"
github_oauth_client_id: "{{ vault_github_oauth_client_id }}"
github_oauth_client_secret: "{{ vault_github_oauth_client_secret }}"

# Security hardening
default_admin_user: "platform-admin"
default_admin_password: "{{ vault_admin_password }}"
remove_kubeadmin: true
remove_selfprovisioning: true

letsencrypt_email: "platform-team@mycompany.com"
```

**Cost**: ~$1,500-2,000/month (HA cluster with multiple node pools)

#### A.3 Edge Testing Cluster (ARM64, GCP)

**Use Case**: ARM-only cluster for automotive edge computing and ECU testing

**Configuration** (`inventory/main.yml`):
```yaml
cloud_provider: "gcp"
cluster_name: "edge-test"
cluster_domain: "edge.automotive.dev"
cluster_version: "4.19.14"
cluster_arch: "aarch64"  # ARM-only cluster
cluster_topology: "default"

installer_architecture: "linux"

# GCP credentials
gcp_service_account_key_file: "$HOME/.gcp/osServiceAccount.json"
gcp_service_account_email: "openshift-sa@automotive-project.iam.gserviceaccount.com"
gcp_project_id: "automotive-project"
gcp_default_region: "us-central1"

# ARM-specific configuration (T2A instances)
# Uses defaults from roles/create-cluster/defaults/main.yml

# Enable Jumpstarter for hardware testing
rhadp_deploy_jumpstart: true
rhadp_deploy_developer_hub: false

# Virtualization not needed for edge testing
setup_baremetal_worker_node: false
setup_virtualization: false

letsencrypt_email: "edge-team@automotive.dev"
```

**Cost**: ~$600-800/month (ARM instances, cost-optimized)

### Appendix B: Role Override Variables

#### B.1 create-cluster Role Overrides

Override instance types and sizes in `inventory/main.yml`:

**AWS Overrides:**
```yaml
# Control plane customization
aws_control_plane_instance_type: "m7g.4xlarge"  # Larger Graviton3
aws_control_plane_instance_count: 5  # 5 control plane nodes

# Worker node customization
aws_worker_instance_type: "c7g.2xlarge"  # Compute-optimized Graviton3
aws_worker_instance_count: 6  # 6 worker nodes
```

**Azure Overrides:**
```yaml
# Control plane customization
azure_control_plane_instance_type: "Standard_D16s_v5"  # 16 vCPU
azure_control_plane_volume_type: "Premium_LRS"

# Worker node customization
azure_worker_instance_type: "Standard_E8s_v5"  # Memory-optimized
azure_worker_volume_type: "StandardSSD_LRS"
```

**GCP Overrides:**
```yaml
# Control plane customization
gcp_control_plane_instance_type: "t2a-standard-16"  # 16 vCPU ARM

# Worker node customization
gcp_worker_instance_type: "t2a-standard-16"  # 16 vCPU ARM
gcp_worker_instance_count: 5
```

**Other Overrides:**
```yaml
# Installer and metadata paths
cluster_ssh_key_dir: "/custom/path/ssh"
cluster_metadata_dir: "/custom/path/metadata"

# OpenShift mirror URL
openshift_clients_url: "https://custom-mirror.example.com/openshift-v4"
```

#### B.2 config-cluster Role Overrides

**ARM Worker Node Customization:**
```yaml
# AWS ARM workers
aws_arm_instance_type: "m7g.2xlarge"
aws_arm_instance_count: 3
aws_arm_volume_size: 256  # GB
aws_arm_volume_type: "io2"  # High-performance SSD

# Azure ARM workers
azure_arm_instance_type: "Standard_D16ps_v5"
azure_arm_instance_count: 2
azure_arm_volume_size: 512
azure_arm_volume_type: "Premium_LRS"

# GCP ARM workers
gcp_arm_instance_type: "t2a-standard-16"
gcp_arm_instance_count: 4
gcp_arm_volume_size: 256
gcp_arm_volume_type: "pd-extreme"
```

**Bare-Metal Node Customization (AWS only):**
```yaml
aws_metal_instance_type: "m6g.metal"
aws_metal_availability_zone: "b"  # Deploy in AZ b
aws_metal_instance_count: 2  # 2 bare-metal nodes
aws_metal_volume_size: 500  # GB
```

**CNV Operator Version Override:**
```yaml
cnv_subscription_channel: "stable"
cnv_subscription_starting_csv: "kubevirt-hyperconverged-operator.v4.19.2"
```

#### B.3 setup-auth Role Overrides

**Keycloak Customization:**
```yaml
# Keycloak namespace override
keycloak_namespace: "custom-auth"

# Keycloak database settings
keycloak_db_user: "keycloak_admin"
keycloak_db_password: "{{ vault_keycloak_db_password }}"
keycloak_db_name: "keycloak"

# Keycloak client configuration
keycloak_client_id: "openshift"
keycloak_client_secret: "{{ vault_keycloak_client_secret }}"
```

**htpasswd Provider Customization:**
```yaml
# Additional users
htpasswd_users:
  - username: "developer1"
    password: "dev1_password"
  - username: "developer2"
    password: "dev2_password"
  - username: "tester"
    password: "test_password"
```

### Appendix C: Glossary

**Technical Terms:**

- **ARM64/aarch64**: 64-bit ARM processor architecture, common in automotive and embedded systems
- **ArgoCD**: GitOps continuous delivery tool for Kubernetes
- **Cluster Operator**: OpenShift component managing specific cluster functionality
- **CSI (Container Storage Interface)**: Standard for exposing storage systems to Kubernetes
- **DevOps**: Development and Operations collaboration methodology
- **Devfile**: YAML file defining developer workspace configuration
- **GitOps**: Infrastructure and application deployment using Git as source of truth
- **Hybrid Cluster**: Cluster with multiple CPU architectures (x86 and ARM)
- **IPI (Installer-Provisioned Infrastructure)**: OpenShift installation mode where installer creates cloud resources
- **Jumpstarter**: Hardware testing automation framework
- **Keycloak**: Open-source identity and access management solution
- **Kubeconfig**: Configuration file for Kubernetes cluster access
- **Manifest List**: Docker/OCI image format supporting multiple architectures
- **OAuth**: Open standard for access delegation and authentication
- **OpenShift**: Enterprise Kubernetes platform by Red Hat
- **Pull Secret**: Credentials for accessing Red Hat container registries
- **RBAC (Role-Based Access Control)**: Kubernetes authorization mechanism
- **Tekton**: Cloud-native CI/CD framework for Kubernetes
- **x86_64/amd64**: 64-bit x86 processor architecture (Intel/AMD)

**OpenShift-Specific:**

- **oc**: OpenShift CLI tool
- **Route**: OpenShift resource exposing service externally (similar to Ingress)
- **Project**: OpenShift namespace with additional annotations and RBAC
- **Operator**: Kubernetes extension for managing complex applications
- **OperatorHub**: Catalog of Kubernetes operators
- **Machine Set**: OpenShift resource defining a set of identical machines
- **Cluster Version Operator (CVO)**: Manages OpenShift cluster version and updates

**Automotive-Specific:**

- **ECU (Electronic Control Unit)**: Embedded system controlling automotive functions
- **RHIVOS**: Red Hat In-Vehicle Operating System
- **Hardware-in-the-Loop (HIL)**: Testing methodology using real hardware

**Cloud Provider:**

- **Availability Zone (AZ)**: Isolated datacenter within a cloud region
- **Virtual Private Cloud (VPC)**: Isolated cloud network
- **Virtual Network (VNet)**: Azure equivalent of VPC
- **Security Group**: Cloud firewall rules for instances
- **Network Security Group (NSG)**: Azure firewall rules
- **Load Balancer**: Distributes traffic across multiple instances
- **Persistent Disk/Volume**: Cloud block storage for data persistence

---

**End of Documentation**

For questions, issues, or contributions, visit [https://github.com/rhadp/rhadp](https://github.com/rhadp/rhadp).
