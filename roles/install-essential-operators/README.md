# install-essential-operators

This Ansible role installs and configures essential operators required for the Red Hat Automotive Development Platform (RHADP) platform.

## Purpose

This role is designed to:
- Automate installation of essential operators
- Configure operator settings and dependencies
- Ensure proper operator integration
- Manage operator lifecycle

## Tasks

This role performs the following tasks:
- Installs Red Hat OpenShift GitOps
- Installs Red Hat OpenShift Pipelines
- Installs HashiCorp Vault
- Installs Red Hat Build of Keycloak
- Configures operator settings
- Verifies operator installations


## References

- [Red Hat OpenShift GitOps Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_gitops/1.16/html/installing_gitops/index)
- [Red Hat OpenShift Pipelines Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_pipelines/1.18/html/installing_and_configuring/index)
- [HashiCorp Vault Kubernetes Documentation](https://developer.hashicorp.com/vault/docs/deploy/kubernetes/vso)
- [Red Hat Build of Keycloak](https://access.redhat.com/products/red-hat-build-of-keycloak)

