# setup-gitops

This Ansible role configures GitOps tooling and infrastructure for the OpenShift cluster.

## Purpose

This role sets up and configures GitOps infrastructure, including:
- Configuring OpenShift GitOps (ArgoCD)
- Setting up GitOps repositories
- Configuring GitOps applications and projects
- Establishing GitOps workflows

## Tasks

This role performs the following tasks:
- Configures OpenShift GitOps instance
- Sets up GitOps repositories
- Creates GitOps applications
- Configures GitOps projects and access
- Establishes GitOps workflows


## References

- [Red Hat OpenShift GitOps Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_gitops/1.16/html/installing_gitops/index)
- [Red Hat OpenShift Pipelines Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_pipelines/1.18/html/installing_and_configuring/index)
- [HashiCorp Vault Kubernetes Documentation](https://developer.hashicorp.com/vault/docs/deploy/kubernetes/vso)
- [Red Hat Build of Keycloak](https://access.redhat.com/products/red-hat-build-of-keycloak)

https://developer.hashicorp.com/vault/docs/deploy/kubernetes/vso
https://developer.hashicorp.com/vault/docs/deploy/kubernetes/vso/openshift
https://access.redhat.com/solutions/5761251

https://access.redhat.com/products/red-hat-build-of-keycloak
https://github.com/keycloak/keycloak

