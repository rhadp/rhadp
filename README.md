# Red Hat Automotive Development Platform

This repository provides Infrastructure as Code (IaC) for deploying the complete [Red Hat Automotive Development Platform](https://github.com/rhadp/rhadp) (RHADP) — a comprehensive, cloud-native development environment specifically designed for automotive software development.

The platform automatically provisions OpenShift clusters across major cloud providers (AWS, Azure, or GCP) with hybrid ARM/x86 architecture support, and pre-configures an integrated suite of development tools to streamline the creation of automotive applications and the Red Hat In-Vehicle Operating System (RHIVOS).  

## Platform overview

Upon deployment, the following components are available out of the box:  

### Core Infrastructure
- OpenShift Container Platform - in various deployment configurations to support different use-cases.
- OpenShift Virtualization - run and manage virtual machines alongside container workloads (optional).
- cert-manager Operator for OpenShift – for certificate management, via Let's Encrypt.
- Red Hat build of Keycloak – for identity and access management.

### Development Platform
- OpenShift Dev Spaces – cloud-based developer workspaces.
- OpenShift GitOps – declarative management of infrastructure and applications.
- OpenShift Pipelines – CI/CD pipelines for building and testing automotive software.
- CI/CD pipeline integration - support for OpenShift Pipelines (Tekton), GitHub Actions and GitLab Runners.
- [Jumpstarter](https://github.com/jumpstarter-dev) - for automated testing on real and virtual hardware with CI/CD integration.  

## Getting started

To deploy the complete platform, follow the steps described in the [deployment guide](docs/deployment.md).

The complete documentation is [here](docs/README.md).

## Contributing

Fork the repository and submit a pull request.

## Development

A list of ideas, open issues etc related to the Red Hat Automotive Development Platform (RHADP) is [here](https://github.com/orgs/rhadp/projects/1).  

Also check the [Issues](https://github.com/rhadp/rhadp/issues) section of the this repository.

## Disclaimer

This is not an officially supported Red Hat product.