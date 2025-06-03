# install-openshift-aws

This Ansible role installs an OpenShift cluster on AWS using the OpenShift installer.

## Purpose

This role is designed to:
- Automate OpenShift cluster installation on AWS
- Handle cluster deployment lifecycle
- Configure initial cluster settings

## Tasks

This role performs the following tasks:
- Creates cluster configuration
- Sets up AWS credentials
- Deploys the cluster
- Waits for cluster readiness
- Configures initial cluster settings


## References

- [OpenShift on AWS Installation Documentation](https://docs.openshift.com/container-platform/latest/installing/installing_aws/installing-aws-default.html)
- [OpenShift Installer Documentation](https://github.com/openshift/installer)
