# Platform Services

## OpenShift Dev Spaces

Cloud-based developer workspaces accessible through a web browser.

**Access URL:**
```
https://devspaces.apps.<cluster_name>.<cluster_domain>
```

**Key Features:**
- Browser-based IDE (Eclipse Che)
- Pre-configured development environments
- Integrated Git operations and debugging
- Workspace defined via devfile


## Developer Hub (Optional)

Unified developer portal for service discovery and collaboration.

**Access URL:**
```
https://developer-hub-<namespace>.apps.<cluster_name>.<cluster_domain>
```

**Deployment:**
```yaml
rhadp_deploy_developer_hub: true
```

**Features:**
- Service catalog
- API documentation
- Project templates
- Plugin ecosystem

## OpenShift GitOps (ArgoCD)

Declarative Git-based infrastructure and application management.

**Access URL:**
```
https://argocd-server-openshift-gitops.apps.<cluster_name>.<cluster_domain>
```

**Key Concepts:**
- **Application**: Kubernetes resources managed as a unit
- **Repository**: Git repo containing manifests
- **Sync**: Process of applying Git state to cluster
- **Sync Policy**: Automatic or manual sync


## OpenShift Pipelines (Tekton)

Cloud-native CI/CD for building, testing, and deploying applications.

**Key Concepts:**
- **Task**: Reusable step (build, test, deploy)
- **Pipeline**: Sequence of Tasks
- **PipelineRun**: Execution instance
- **Trigger**: Automatic execution on events


## Jumpstarter (Optional)

Automated testing on real and virtual hardware for automotive ECU testing.

**Deployment:**
```yaml
rhadp_deploy_jumpstart: true
```

**Features:**
- Remote device management
- QEMU exporter for virtual hardware
- CI/CD integration
- Real hardware support (via exporters)

**Use Case: Automotive ECU Testing**
1. Build software in OpenShift Pipelines
2. Trigger Jumpstarter test on virtual/real hardware
3. Collect test results
4. Deploy on success
