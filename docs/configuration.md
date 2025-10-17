# Configuration Reference

This document provides a complete reference for all configuration variables available in `inventory/main.yml`.

## Cluster Configuration

#### Cloud Provider and Version

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cloud_provider` | String | `aws` | Yes | Cloud platform: `aws`, `azure`, `gcp` |
| `cluster_version` | String | `4.19.14` | Yes | OpenShift version (must match available version on Red Hat mirror) |
| `installer_architecture` | String | `mac-arm64` | Yes | Local machine architecture: `mac`, `mac-arm64`, `linux`, `linux-arm64` |

**Example:**
```yaml
cloud_provider: "aws"
cluster_version: "4.19.14"
installer_architecture: "linux"
```

#### Cluster Architecture

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cluster_arch` | String | `x86_64` | Yes | Node architecture: `x86_64` (all x86), `aarch64` (all ARM), `multi` (hybrid, AWS/Azure only) |
| `control_plane_arch` | String | `amd64` | Conditional | Control plane architecture (only when `cluster_arch=multi`): `amd64`, `arm64` |

**Example:**
```yaml
cluster_arch: "multi"
control_plane_arch: "amd64"
```

#### Cluster Topology

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cluster_topology` | String | `default` | Yes | Deployment pattern: `default` (3 control + workers), `compact` (control nodes also run workloads), `metal` (bare-metal) |

#### Cluster Naming and DNS

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cluster_name` | String | `rhadp` | Yes | Cluster identifier (DNS-compatible: lowercase, alphanumeric, hyphens) |
| `cluster_domain` | String | `rhadp.example.com` | Yes | Base FQDN for cluster routes |
| `cluster_toplevel_domain` | String | `example.com` | Yes | Domain for default user emails |

#### Storage Configuration

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `cluster_default_storage_class` | String | `gp3-csi` | Yes | Default storage class (AWS: `gp3-csi`, Azure: `managed-csi`, GCP: `standard-csi`) |

#### Pull Secret

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `pull_secret_file` | String | `inventory/pull-secret.txt` | Yes | Path to Red Hat pull secret JSON file |

#### Cluster Deployment Options

| Parameter | Type | Default | Required | Values/Notes |
|-----------|------|---------|----------|--------------|
| `setup_arm_worker_nodes` | Boolean | `false` | No | Create additional ARM64 worker pool |
| `setup_baremetal_worker_node` | Boolean | `false` | No | Create bare-metal ARM node (AWS m6g.metal only) |
| `setup_virtualization` | Boolean | `false` | No | Install OpenShift Virtualization (requires bare-metal node) |

## Cloud Provider Credentials

#### AWS Variables

| Parameter | Type | Required | Values/Notes |
|-----------|------|----------|--------------|
| `aws_access_key_id` | String | Yes (AWS) | AWS IAM access key - **Sensitive** |
| `aws_secret_access_key` | String | Yes (AWS) | AWS IAM secret key - **Sensitive, use Ansible Vault** |
| `aws_default_region` | String | Yes (AWS) | AWS region (e.g., `us-east-2`) |

#### Azure Variables

| Parameter | Type | Required | Values/Notes |
|-----------|------|----------|--------------|
| `azure_guid` | String (UUID) | Yes (Azure) | Azure GUID identifier |
| `azure_client_id` | String (UUID) | Yes (Azure) | Service principal client ID - **Sensitive** |
| `azure_password` | String | Yes (Azure) | Service principal password - **Sensitive, use Ansible Vault** |
| `azure_tenant` | String | Yes (Azure) | Azure AD tenant |
| `azure_subscription` | String (UUID) | Yes (Azure) | Azure subscription ID |
| `azure_resource_group` | String | Yes (Azure) | Resource group name |
| `azure_default_region` | String | Yes (Azure) | Azure region (e.g., `eastus`) |

#### GCP Variables

| Parameter | Type | Required | Values/Notes |
|-----------|------|----------|--------------|
| `gcp_service_account_key_file` | String (path) | Yes (GCP) | Path to service account JSON key - **Sensitive** |
| `gcp_service_account_email` | String (email) | Yes (GCP) | Service account email |
| `gcp_project_id` | String | Yes (GCP) | GCP project ID |
| `gcp_organization_id` | String | No | Numeric organization ID |
| `gcp_default_region` | String | Yes (GCP) | GCP region (e.g., `us-central1`) |

#### GitHub Integration

| Parameter | Type | Required | Values/Notes |
|-----------|------|----------|--------------|
| `github_endpoint` | String (URL) | No | GitHub API endpoint (default: `https://github.com`) |
| `github_org` | String | No | GitHub organization name |
| `github_api_pat` | String | No | Personal Access Token - **Sensitive, use Ansible Vault** |
| `github_git_client_id` | String | No | OAuth app client ID for Git - **Sensitive** |
| `github_git_client_secret` | String | No | OAuth app client secret for Git - **Sensitive** |
| `github_oauth_client_id` | String | No | OAuth app client ID for Keycloak - **Sensitive** |
| `github_oauth_client_secret` | String | No | OAuth app client secret for Keycloak - **Sensitive** |

#### Certificate Management

| Parameter | Type | Required | Values/Notes |
|-----------|------|----------|--------------|
| `letsencrypt_email` | String (email) | Yes | Email for Let's Encrypt notifications |

## Platform Deployment Options

#### Feature Flags

| Parameter | Type | Default | Values/Notes |
|-----------|------|---------|--------------|
| `rhadp_deploy_developer_hub` | Boolean | `false` | Deploy Red Hat Developer Hub |
| `rhadp_deploy_jumpstart` | Boolean | `false` | Deploy Jumpstarter operator |

#### Platform Configuration

| Parameter | Type | Default | Values/Notes |
|-----------|------|---------|--------------|
| `rhadp_prefix` | String | `rhadp` | Prefix for platform namespaces |
| `rhadp_gitops_repo_url` | String (URL) | `https://github.com/rhadp/rhadp-manifests` | Git repository for ArgoCD manifests |
| `rhadp_gitops_repo_revision` | String | `develop` | Git branch, tag, or commit SHA |

#### Platform Namespaces

| Parameter | Type | Default |
|-----------|------|---------|
| `platform_devspaces_namespace` | String | `{{ rhadp_prefix }}-devspaces` |
| `platform_developer_hub_namespace` | String | `{{ rhadp_prefix }}-devhub` |
| `platform_builder_namespace` | String | `{{ rhadp_prefix }}-builder` |
| `platform_jumpstarter_namespace` | String | `{{ rhadp_prefix }}-jumpstarter` |

## Platform Internals

#### Admin User Configuration

| Parameter | Type | Default | Values/Notes |
|-----------|------|---------|--------------|
| `default_admin_user` | String | `admin` | Administrator username - **Change for production** |
| `default_admin_password` | String | `openshift` | Administrator password - **MUST CHANGE for production** |
| `default_admin_email` | String (email) | `admin@example.com` | Administrator email |

#### Test User Configuration

| Parameter | Type | Default | Values/Notes |
|-----------|------|---------|--------------|
| `default_nobody_user` | String | `nobody` | Unprivileged test user name |
| `default_nobody_password` | String | `nobody` | Unprivileged test user password |

#### Security Settings

| Parameter | Type | Default | Values/Notes |
|-----------|------|---------|--------------|
| `remove_kubeadmin` | Boolean | `true` | Remove default kubeadmin user - **Recommended for production** |
| `remove_selfprovisioning` | Boolean | `true` | Prevent users from creating own namespaces |

#### Keycloak Configuration

| Parameter | Type | Default | Values/Notes |
|-----------|------|---------|--------------|
| `keycloak_db_user` | String | `admin` | PostgreSQL username - **Change for production** |
| `keycloak_db_password` | String | `openshift` | PostgreSQL password - **MUST CHANGE, use Ansible Vault** |
| `keycloak_client_secret` | String | `openshiftkc` | OAuth client secret - **MUST CHANGE** |

#### Other Settings

| Parameter | Type | Default | Values/Notes |
|-----------|------|---------|--------------|
| `remove_installer_binary` | Boolean | `true` | Remove installer files after deployment |
