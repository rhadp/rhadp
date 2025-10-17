# Reference

## Official OpenShift Documentation

**OpenShift Container Platform 4.19:**
- [OCP 4.19 Documentation Portal](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19)
- [Installing on AWS](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/installing_on_aws/index)
- [Installing on Azure](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_azure/index)
- [Installing on GCP](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_gcp/index)
- [Release Notes](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/release_notes/index)

**Platform Services:**
- [OpenShift GitOps Documentation](https://docs.openshift.com/gitops/latest/)
- [OpenShift Pipelines Documentation](https://docs.openshift.com/pipelines/latest/)
- [OpenShift Dev Spaces Documentation](https://docs.redhat.com/en/documentation/red_hat_openshift_dev_spaces/)
- [OpenShift Virtualization Documentation](https://docs.openshift.com/container-platform/4.19/virt/about_virt/about-virt.html)

## Related Projects

**RHADP Repositories:**
- [RHADP Repository](https://github.com/rhadp/rhadp) - Infrastructure as Code
- [RHADP Manifests Repository](https://github.com/rhadp/rhadp-manifests) - GitOps manifests

**Related Technologies:**
- [Jumpstarter Project](https://github.com/jumpstarter-dev) - Hardware testing automation
- [Ansible Documentation](https://docs.ansible.com/) - Automation engine
- [Tekton Documentation](https://tekton.dev/docs/) - CI/CD framework

**Red Hat Resources:**
- [Red Hat OpenShift Cluster Manager](https://console.redhat.com/openshift) - Cluster management
- [Red Hat Container Catalog](https://catalog.redhat.com/software/containers/explore) - Certified images
- [Red Hat Developer](https://developers.redhat.com/) - Developer resources

## Ansible Roles Documentation

**Core Roles:**
- `create-cluster`: Cluster bootstrapping
- `config-cluster`: Topology configuration
- `gather-cluster-facts`: Cluster discovery
- `install-operator`: Operator installation

**Platform Roles:**
- `setup-auth`: Authentication and Keycloak
- `setup-cert-manager`: Certificate management
- `setup-devops`: GitOps and Pipelines
- `setup-platform`: Dev Spaces and Developer Hub
- `setup-jumpstarter`: Jumpstarter operator

**Lifecycle Roles:**
- `lifecycle-cluster`: Start/stop operations

**Builder Roles:**
- `create-builder`: Builder VM creation
- `setup-runner`: GitHub/GitLab runner

## Community and Support

**Project Resources:**
- **GitHub Issues**: [https://github.com/rhadp/rhadp/issues](https://github.com/rhadp/rhadp/issues)
- **Project Board**: [https://github.com/orgs/rhadp/projects/1](https://github.com/orgs/rhadp/projects/1)
- **Contributing**: See CONTRIBUTING.md

**Red Hat Support:**
- **Red Hat Customer Portal**: [https://access.redhat.com](https://access.redhat.com)
- For Red Hat subscription customers

**Disclaimer:**
This is not an officially supported Red Hat product. For production use, consult Red Hat support and follow enterprise deployment best practices.
