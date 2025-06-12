# RHADP Bootstrap - Development Roadmap

## Overview
This document outlines the development roadmap for the RHADP (Red Hat Advanced Development Platform) Bootstrap project, organized by priority and completion status.

---

## 🚨 Critical Infrastructure (Must Complete)

### Cluster Infrastructure & Core Components

#### 1. Multi-Cloud Installation Support
- [ ] **Azure Installation**
  - [ ] Implement Azure installation
  - [ ] Test and validate Azure deployment
  - [ ] Document Azure-specific requirements and limitations
  - [ ] Create Azure-specific troubleshooting guide

#### 2. Identity & Access Management (IAM)
- [ ] **RBAC Framework Design**
  - [ ] Define user personas and access patterns
  - [ ] Document default OpenShift groups and role bindings
  - [ ] Implement role-based access controls

- [ ] **Keycloak Integration**
  - [ ] Configure user/group synchronization between Keycloak and OpenShift
  - [ ] Set up additional Identity Providers:
    - [ ] GitHub OAuth integration
    - [ ] GitLab OAuth integration
  - [ ] Create user onboarding documentation

#### 3. Security & Secrets Management
- [ ] **Vault Configuration**
  - [ ] Implement automated Vault preparation and unsealing
  - [ ] Integrate Vault with OpenShift service accounts
  - [ ] Create backup and disaster recovery procedures

#### 4. Network & DNS Configuration
- [ ] **Custom Domain Setup**
  - [ ] Replace Route53 dependency with custom DNS solution
  - [ ] Update cert-operator/Let's Encrypt configuration
  - [ ] Test certificate renewal processes

#### 5. GitOps Integration
- [ ] **OpenShift GitOps SSO**
  - [ ] Fix Keycloak integration with ArgoCD
  - [ ] Implement proper RBAC for GitOps applications
  - [ ] Configure application visibility based on user permissions
  - [ ] Reference: [Red Hat OpenShift GitOps SSO Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_gitops/1.16/html/access_control_and_user_management/configuring-sso-for-argo-cd-using-dex)


---

## 🔧 Platform Enhancements (High Priority)

### Hardware & Infrastructure Optimization

#### 1. ARM Architecture Support
- [ ] **ARM Bare-Metal Configuration**
  - [ ] Configure OpenShift Virt operator for ARM bare-metal
  - [ ] Test VM workloads on ARM architecture

#### 2. GPU Support
- [ ] **NVIDIA GPU Integration**
  - [ ] Configure x86 machineset with NVIDIA GPU instance types
  - [ ] Install and configure NVIDIA operator


---

## 🎯 Development Platform MVP

### Core Development Tools

#### 1. Self-Managed GitLab
- [ ] **Installation & Configuration**
  - [ ] Deploy GitLab using [official installation methods](https://docs.gitlab.com/install/install_methods/)
  - [ ] Configure Keycloak as [OIDC provider](https://docs.gitlab.com/integration/openid_connect_provider/)
  - [ ] Set up GitLab roles, groups, and permissions
  - [ ] Import existing GitOps/Infrastructure-as-code repositories
  - [ ] Optional: Configure GitLab runners on OpenShift
  - [ ] Set up [GitLab Workspaces](https://docs.gitlab.com/user/workspace/configuration/) (DevSpaces alternative)

#### 2. OpenShift DevSpaces
- [ ] **Operator Installation & Setup**
  - [ ] Install DevSpaces operator
  - [ ] Configure Keycloak authentication
  - [ ] Set up automatic authentication to hosted GitLab
  - [ ] Customize workspace templates (remove Java Spring Boot, etc.)
  - [ ] Configure workspace resource limits and policies

#### 3. OpenShift Developer Hub
- [ ] **Core Configuration**
  - [ ] Install Developer Hub operator
  - [ ] Configure [Keycloak authentication](https://docs.redhat.com/en/documentation/red_hat_developer_hub/1.6/html/authentication_in_red_hat_developer_hub/assembly-authenticating-with-rhbk)
  - [ ] Set up [RBAC plugin](https://docs.redhat.com/en/documentation/red_hat_developer_hub/1.6/html/authorization_in_red_hat_developer_hub/index)
  - [ ] Configure essential plugins (GitOps, GitLab)
  - [ ] Disable unnecessary plugins and templates
  - [ ] Import RHIVOS templates for OS image building
  - [ ] Configure automatic authentication to GitLab and DevSpaces

#### 4. Jumpstarter Operator
- [ ] **Deployment & Configuration**
  - [ ] Install Jumpstarter operator
  - [ ] Configure inventory exporters
  - [ ] Set up default exporter configuration in DevSpaces
  - [ ] Create exporter templates and documentation

---

## 🤔 Architecture Decisions (To Be Resolved)

### Container Registry Strategy
**Current Decision**: OpenShift Quay excluded due to ODF dependency

**Options to Evaluate**:
- [ ] **OpenShift Registry**
  - Pros: Built-in, no additional dependencies
  - Cons: Limited features, no external access
- [ ] **External Registries**
  - AWS ECR, Azure Container Registry, GitHub Container Registry
  - Pros: Managed service, feature-rich
  - Cons: External dependency, potential costs
- [ ] **GitLab Container Registry**
  - Pros: Integrated with GitLab, no additional setup
  - Cons: Limited to GitLab projects

### Object Storage Solution
**Current Consideration**: Evaluate "Garage" due to MinIO license changes

**Options to Evaluate**:
- [ ] **Garage** ([GitHub](https://git.deuxfleurs.fr/Deuxfleurs/garage))
  - Pros: Open source, S3-compatible
  - Cons: Newer project, limited community
- [ ] **MinIO** (if license acceptable)
  - Pros: Mature, widely adopted
  - Cons: License concerns


### Artifact Management
**Options to Evaluate**:
- [ ] **JFrog Artifactory**
  - Pros: Enterprise-grade, comprehensive
  - Cons: Licensing costs, complexity
- [ ] **Nexus Repository**
  - Pros: Open source, mature
  - Cons: Self-hosted maintenance


---

## 📋 Task Tracking

### Priority Levels
- **P0**: Critical path, blocking other work
- **P1**: High priority, should complete soon
- **P2**: Medium priority, nice to have
- **P3**: Low priority, future consideration

### Status Tracking
- [ ] Not Started
- [🔄] In Progress
- [✅] Completed
- [❌] Blocked
- [⏸️] On Hold

---

## 📚 Resources & References

### Documentation Links
- [OpenShift Post-Installation Configuration](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/postinstallation_configuration/index)
- [Red Hat OpenShift GitOps SSO](https://docs.redhat.com/en/documentation/red_hat_openshift_gitops/1.16/html/access_control_and_user_management/configuring-sso-for-argo-cd-using-dex)
- [GitLab Installation Methods](https://docs.gitlab.com/install/install_methods/)
- [GitLab OIDC Integration](https://docs.gitlab.com/integration/openid_connect_provider/)
- [GitLab Workspaces](https://docs.gitlab.com/user/workspace/configuration/)
- [Developer Hub Authentication](https://docs.redhat.com/en/documentation/red_hat_developer_hub/1.6/html/authentication_in_red_hat_developer_hub/assembly-authenticating-with-rhbk)
- [Developer Hub RBAC](https://docs.redhat.com/en/documentation/red_hat_developer_hub/1.6/html/authorization_in_red_hat_developer_hub/index)
- [Garage Object Storage](https://git.deuxfleurs.fr/Deuxfleurs/garage)

### Next Steps
1. Prioritize critical infrastructure items
2. Create detailed implementation plans for each component
3. Set up project milestones and timelines
4. Establish testing and validation procedures
5. Create user documentation and guides
