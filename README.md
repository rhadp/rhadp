# Red Hat Automotive Development Platform

This repository contains "Infrastructure as Code (IaC)" for deploying the **Red Hat Automotive Development Platform (RHADP)** — a cloud-native development environment purpose-built for automotive software development.  

The setup automatically provisions multi-cloud OpenShift clusters (on AWS, Azure, or GCP) with a hybrid ARM/x86 architecture, and installs a curated set of tools to accelerate the development of automotive applications and the **Red Hat In-Vehicle Operating System (RHIVOS)**.  

## Platform overview

Upon deployment, the following core components are available out of the box:  
- OpenShift Dev Spaces – cloud-based developer workspaces  
- OpenShift GitOps – declarative management of infrastructure and applications  
- OpenShift Pipelines – CI/CD pipelines for building and testing automotive software  

The platform also installs following key services:  
- cert-manager Operator for OpenShift – for certificate management (via Let’s Encrypt)  
- Red Hat build of Keycloak – for identity and access management  

RHADP supports advanced use cases to address automotive-specific needs:  
- Building and testing RHIVOS images directly in the cloud
- Project [Jumpstarter](https://jumpstarter.dev) for accessing automotive development boards from the cloud (Hardware-in-the-loop testing)
- OpenShift Virtualization for running isolated RHIVOS virtual machines on ARM bare-metal node (Software-in-the-loop testing)

## Getting started

To deploy RHADP, follow the steps described in the [deployment guide](docs/deployment.md).

The complete set of documentation is [here](docs/README.md).

## Contributing

Fork the repository and submit a pull request.

## Development

A list of ideas, open issues etc is [here](https://github.com/orgs/rhadp/projects/1). Also check the [Issues](https://github.com/rhadp/rhadp-bootstrap/issues) section of the this repository.

## Disclaimer

This is not an officially supported Red Hat product.