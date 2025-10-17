# Technologies and Components

## Core Infrastructure

### OpenShift Container Platform 4.19
- Enterprise Kubernetes distribution for container orchestration
- Application platform for deploying containerized workloads
- Support for multiple deployment topologies (default, compact, bare-metal)
- Built-in monitoring, logging, and observability

### OpenShift Virtualization (Optional)
- Virtual machine management alongside container workloads
- Based on KubeVirt technology
- Unified management of VMs and containers
- Requires bare-metal nodes with nested virtualization support

### cert-manager Operator for OpenShift
- Automated X.509 certificate management
- Let's Encrypt integration for automatic TLS certificate provisioning
- Certificate lifecycle management (issuance, renewal, revocation)
- Support for ingress and route certificate automation

## Identity and Access Management

### Red Hat build of Keycloak
- Enterprise-grade identity and access management solution
- OAuth 2.0 and OpenID Connect (OIDC) provider
- User federation and single sign-on (SSO) capabilities
- Integration with external identity providers (GitHub, GitLab, LDAP)

### htpasswd OAuth Provider
- Local user authentication mechanism
- File-based user credential storage
- Admin and service account management
- Bootstrap authentication before external providers are configured

## Development Platform

### OpenShift Dev Spaces
- Cloud-based developer workspaces accessible via web browser
- Based on Eclipse Che technology
- Pre-configured development environments for consistent team workflows
- Support for devfile-based workspace definitions
- Integrated terminal, debugger, and Git tools

### Developer Hub (Optional)
- Unified developer portal powered by Red Hat Developer Hub
- Service catalog for discovering and using platform services
- API documentation and technical resources
- Team collaboration and project templates

### OpenShift GitOps (ArgoCD)
- Declarative infrastructure and application management
- GitOps workflows for continuous delivery
- Automated synchronization between Git repositories and cluster state
- Multi-cluster and multi-tenant support
- Rollback and drift detection capabilities

### OpenShift Pipelines (Tekton)
- Cloud-native CI/CD pipeline framework
- Kubernetes-native build automation
- Reusable pipeline tasks and components
- Integration with Git webhooks for automated builds
- Support for parallel execution and complex workflows

## Testing and Automation

### Jumpstarter (Optional)
- Automated testing framework for real and virtual hardware
- CI/CD integration for hardware-in-the-loop testing
- QEMU exporter support for virtual device testing
- Remote device management and orchestration
- Ideal for automotive ECU and embedded system testing

## Infrastructure as Code

### Ansible
- Automation engine for deployment and configuration management
- Role-based modular architecture for extensibility
- Idempotent playbook execution
- Support for cloud provider APIs (AWS, Azure, GCP)

### OpenShift Installer (openshift-install)
- Official Red Hat installer for OpenShift cluster bootstrapping
- Installer-provisioned infrastructure (IPI) mode
- Multi-cloud provider support
- Declarative cluster configuration via install-config.yaml
