# Red Hat Automotive Development Platform

This repository contains "Infrastructure as Code (IaC)" for deploying Red Hat OpenShift with a cluster topology, needed to support the 
the [Red Hat Automotive Development Platform](https://github.com/rhadp/rhadp-platform) (RHADP) — a cloud-native development environment 
purpose-built for automotive software development.  

This setup automatically provisions an OpenShift clusters (on AWS, Azure, or GCP) with a hybrid ARM/x86 architecture, 
and installs a curated set of tools to accelerate the development of automotive applications and the Red Hat In-Vehicle Operating System (RHIVOS).  

## Cluster overview

Upon deployment, the following core components are available out of the box:  
- OpenShift Container Platform - in various deplyoment configurations to support different use-cases
- OpenShift Virtualization - run and manage virtual machines alongside container workloads (optional)
- cert-manager Operator for OpenShift – for certificate management (via Let’s Encrypt)  
- Red Hat build of Keycloak – for identity and access management  

## Getting started

To deploy the cluster, follow the steps described in the [deployment guide](docs/deployment.md).

The complete documentation is [here](docs/README.md).

## Contributing

Fork the repository and submit a pull request.

## Development

A list of ideas, open issues etc is [here](https://github.com/orgs/rhadp/projects/1). Also check the [Issues](https://github.com/rhadp/rhadp-bootstrap/issues) section of the this repository.

## Disclaimer

This is not an officially supported Red Hat product.