# Prerequisites

## System Requirements

### Local Workstation Requirements
- **Operating System**: macOS, Linux, or Windows with WSL2
- **Python**: Version 3.12 or later
- **Git**: Version 2.0 or later

## Cloud Provider Requirements

### AWS Requirements

**Reference**: [Installing on AWS - Red Hat OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/installing_on_aws/index)

**Additional Considerations**
- Support for ARM instances (Graviton2/3) in selected regions
- Bare-metal instance availability (e.g. m6g.metal) if using OpenShift Virtualization
- Availability zone requirements: 3 AZs recommended for high availability

### Azure Requirements

**Reference**: [Installing on Azure - Red Hat OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_azure/index)

**Additional Considerations**
- ARM instance support: Standard_D8ps_v5 and similar Ampere-based VMs
- Regional availability: Verify ARM VM availability in target region
- Network security: Default outbound access considerations (Azure retiring default outbound after Sept 2025)

### GCP Requirements

**Reference**: [Installing on GCP - Red Hat OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_gcp/index)

**Additional Considerations**
- ARM support: T2A instance family (Ampere Altra) availability by region
- Confidential computing: AMD SEV and Intel TDX support in OpenShift 4.19
- Regional quotas: Verify quota availability in target region

## Red Hat Account

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

## Additional Requirements (Optional)

### GitHub Integration (Optional)
- GitHub account (personal or organization)
- GitHub Personal Access Token (PAT) with scopes:
  - `repo` (full control of private repositories)
  - `admin:org` (for organization integration)
  - `workflow` (for GitHub Actions integration)
- GitHub OAuth App (for Keycloak integration):
  - Client ID and Client Secret
  - Authorization callback URL: `https://keycloak-<namespace>.apps.<cluster_domain>/auth/realms/master/broker/github/endpoint`

### GitLab Integration (Optional)
- GitLab account (gitlab.com or self-hosted)
- GitLab Personal Access Token with scopes:
  - `api` (access to GitLab API)
  - `read_repository` and `write_repository`
- GitLab OAuth Application (for Keycloak integration)

### Let's Encrypt
- Valid email address for certificate expiration notifications
- Email must be accessible for emergency certificate issues
- Recommended: Use a team or group email, not personal
