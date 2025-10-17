# Deployment Guide

## Environment Setup

### Clone the Repository

```bash
git clone https://github.com/rhadp/rhadp.git
cd rhadp
```

Fork the repository if you plan to customize playbooks, roles, or configuration templates.

### Setup Python Virtual Environment

```bash
# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate  # macOS/Linux

# Install dependencies
pip install -r requirements.txt

# Install Azure collection (if deploying to Azure)
ansible-galaxy collection install azure.azcollection --force
pip install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements.txt
```

### Verify Ansible Installation

```bash
ansible --version  # Should be 2.16 or later
ansible-galaxy collection list
```

## Configuration

### Copy Example Configuration

```bash
cp inventory/main.yml.example inventory/main.yml
```

### Obtain Red Hat Pull Secret

1. Visit [Red Hat OpenShift Cluster Manager](https://console.redhat.com/openshift/create)
2. Log in with your Red Hat account
3. Select the OpenShift [cluster type](https://console.redhat.com/openshift/create)
4. Download the pull secret (JSON file)
5. Save to `inventory/pull-secret.txt`

### Additional Requirements (Optional)

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


### Configure Cloud Provider Credentials

Edit `inventory/main.yml` and configure your cloud provider:

**AWS:**
```yaml
cloud_provider: "aws"
aws_access_key_id: "AKIAIOSFODNN7EXAMPLE"
aws_secret_access_key: "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
aws_default_region: "us-east-2"
```

**Azure:**
```yaml
cloud_provider: "azure"
azure_client_id: "00000000-0000-0000-0000-000000000000"
azure_password: "your-service-principal-password"
azure_tenant: "your-tenant.onmicrosoft.com"
azure_subscription: "00000000-0000-0000-0000-000000000000"
azure_resource_group: "rhadp-rg"
azure_default_region: "eastus"
```

**GCP:**
```yaml
cloud_provider: "gcp"
gcp_service_account_key_file: "$HOME/.gcp/osServiceAccount.json"
gcp_service_account_email: "openshift-sa@your-project.iam.gserviceaccount.com"
gcp_project_id: "your-project-id"
gcp_default_region: "us-central1"
```

### Configure Cluster Settings

```yaml
cluster_name: "rhadp"
cluster_domain: "rhadp.example.com"
cluster_version: "4.19.14"
cluster_arch: "x86_64"  # Options: x86_64, aarch64, multi
cluster_topology: "default"  # Options: default, compact, metal
installer_architecture: "mac-arm64"  # Options: mac, mac-arm64, linux, linux-arm64
```

### Configure Optional Features

**Multi-Architecture:**
```yaml
setup_arm_worker_nodes: true
setup_baremetal_worker_node: true  # AWS only, for virtualization
setup_virtualization: true
```

**Platform Services:**
```yaml
rhadp_deploy_developer_hub: true
rhadp_deploy_jumpstart: true
rhadp_gitops_repo_url: "https://github.com/rhadp/rhadp-manifests"
```

**Security:**
```yaml
default_admin_password: "ChangeMe123!"
letsencrypt_email: "devops-team@example.com"
remove_kubeadmin: true
remove_selfprovisioning: true
```

## Deploy the Platform

### Run Full Deployment

```bash
source venv/bin/activate
ansible-playbook -i inventory/ 0_all.yml
```

**Expected Duration: 60-90 minutes**

This playbook executes:
1. Cluster fact gathering and validation
2. OpenShift installer download
3. SSH key generation
4. Cloud infrastructure creation
5. OpenShift cluster bootstrap
6. Topology configuration (ARM nodes, bare-metal nodes)
7. cert-manager and Let's Encrypt setup
8. Keycloak deployment
9. GitOps and Pipelines installation
10. Development platform services deployment
11. RBAC configuration

### Monitor Deployment Progress

**Watch OpenShift Installer Logs:**
```bash
tail -f $HOME/.openshift/<cluster-name>-<cloud-provider>/.openshift_install.log
```

**Common Log Messages:**
- `Waiting for bootstrap complete`: Control plane initializing
- `Waiting for cluster operators`: Operators deploying
- `Cluster is initialized`: Bootstrap complete

### Deployment Troubleshooting

If deployment fails:

1. Check installer logs: `cat $HOME/.openshift/<cluster-name>-<cloud-provider>/.openshift_install.log`
2. Verify cloud credentials in `inventory/main.yml`
3. Check service quotas in cloud provider console
4. Verify pull secret is valid

**Retry Deployment:**
```bash
# Destroy partial resources first
ansible-playbook -i inventory/ 9_destroy_cluster.yml

# Retry deployment
ansible-playbook -i inventory/ 0_all.yml
```

## Access the Platform

### Cluster Access URLs

**OpenShift Console:**
```
https://console-openshift-console.apps.<cluster_name>.<cluster_domain>
```

**OpenShift API:**
```
https://api.<cluster_name>.<cluster_domain>:6443
```

**Kubeconfig Location:**
```
$HOME/.openshift/<cluster-name>-<cloud-provider>/auth/kubeconfig
```

### Default Credentials

**Admin User:**
- Username: `admin` (or value of `default_admin_user`)
- Password: `openshift` (or value of `default_admin_password`)

**Change default password immediately after first login.**

### CLI Access

```bash
# Set KUBECONFIG
export KUBECONFIG=$HOME/.openshift/<cluster-name>-<cloud-provider>/auth/kubeconfig

# Verify authentication
oc whoami

# Login as admin
oc login -u admin -p openshift
```

## Verify Deployment

```bash
# Check nodes
oc get nodes
oc get nodes -o wide

# Verify cluster operators
oc get co

# Check all pods
oc get pods -A

# Verify platform components
oc get pods -n rhadp-devspaces
oc get pods -n openshift-gitops
oc get pods -n openshift-pipelines

# Check certificates
oc get certificate -A

# Review cluster version
oc get clusterversion
```

**Expected Healthy State:**
- All nodes in `Ready` status
- All cluster operators `AVAILABLE=True`, `DEGRADED=False`
- All pods in `Running` or `Completed` status
- Certificates issued and valid
