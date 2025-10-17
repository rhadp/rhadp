# Appendices

## Example Configurations

### Minimal Development Cluster (x86_64, AWS)

**Use Case:** Small development cluster for testing

```yaml
cloud_provider: "aws"
cluster_name: "dev"
cluster_domain: "dev.example.com"
cluster_version: "4.19.14"
cluster_arch: "x86_64"
cluster_topology: "compact"

aws_access_key_id: "{{ lookup('env', 'AWS_ACCESS_KEY_ID') }}"
aws_secret_access_key: "{{ lookup('env', 'AWS_SECRET_ACCESS_KEY') }}"
aws_default_region: "us-east-2"

setup_arm_worker_nodes: false
setup_virtualization: false
rhadp_deploy_developer_hub: false
rhadp_deploy_jumpstart: false

letsencrypt_email: "devops@example.com"
```


### Production Cluster (Multi-Arch, Azure)

**Use Case:** Production automotive development platform

```yaml
cloud_provider: "azure"
cluster_name: "rhadp-prod"
cluster_domain: "rhadp.mycompany.com"
cluster_version: "4.19.14"
cluster_arch: "multi"
control_plane_arch: "amd64"

azure_client_id: "{{ vault_azure_client_id }}"
azure_password: "{{ vault_azure_password }}"
azure_tenant: "mycompany.onmicrosoft.com"
azure_subscription: "{{ vault_azure_subscription }}"
azure_resource_group: "rhadp-production-rg"
azure_default_region: "eastus"

setup_arm_worker_nodes: true
rhadp_deploy_developer_hub: true
rhadp_deploy_jumpstart: true

github_org: "mycompany"
github_api_pat: "{{ vault_github_pat }}"

default_admin_password: "{{ vault_admin_password }}"
remove_kubeadmin: true
remove_selfprovisioning: true
```
