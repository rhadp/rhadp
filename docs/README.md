# Red Hat Automotive Development Platform

The Red Hat Automotive Development Platform (RHADP) is an Infrastructure as Code solution that automates deployment of a complete cloud-native development environment for automotive software development and RHIVOS.

## Documentation

### Getting Started
- **[Prerequisites](prerequisites.md)** - Cloud provider requirements, Red Hat account, credentials
- **[Deployment Guide](deployment.md)** - Step-by-step deployment instructions

### Configuration
- **[Configuration Reference](configuration.md)** - Complete inventory variable reference

### Additional Resources
- **[Multi-Cloud Deployment](multi-cloud.md)** - AWS, Azure, and GCP deployment details
- **[Reference](reference.md)** - Official documentation, community resources
- **[Appendices](appendices.md)** - Example configurations, role overrides, glossary

## Architecture Overview

RHADP deploys a layered architecture:

**Infrastructure Layer**
- OpenShift Container Platform 4.19
- Cloud provider infrastructure (AWS, Azure, or GCP)
- Multi-architecture compute nodes (x86_64, ARM64, or hybrid)
- OpenShift Virtualization (optional)

**Platform Services Layer**
- Identity and Access Management (Keycloak + OAuth)
- Certificate Management (cert-manager with Let's Encrypt)
- GitOps (OpenShift GitOps/ArgoCD)
- CI/CD (OpenShift Pipelines/Tekton)

**Development Tools Layer**
- OpenShift Dev Spaces (browser-based IDE)
- Developer Hub (optional, service catalog)
- Jumpstarter (optional, hardware testing)
- GitHub/GitLab runner integration

## Key Features

- **Multi-Cloud**: Deploy on AWS, Azure, or GCP with identical workflows
- **Multi-Architecture**: Native support for x86_64, ARM64, and hybrid clusters
- **Automated Deployment**: Single-command deployment of complete platform
- **Pre-Integrated Toolchain**: GitOps, Pipelines, Dev Spaces out-of-the-box
- **Hardware Testing**: Jumpstarter integration for automated ECU testing
- **Lifecycle Management**: Start, stop, reconfigure, and destroy with Ansible playbooks

## Use Cases

- Automotive software development and RHIVOS development
- Multi-architecture development (ARM and x86 workloads)
- Hardware testing with real and virtual automotive hardware
- CI/CD automation for automotive software

## Support

- **GitHub Issues**: [https://github.com/rhadp/rhadp/issues](https://github.com/rhadp/rhadp/issues)
- **Project Board**: [https://github.com/orgs/rhadp/projects/1](https://github.com/orgs/rhadp/projects/1)

**Disclaimer**: This is not an officially supported Red Hat product. For production use, consult Red Hat sales.

## License

See LICENSE file for details.
