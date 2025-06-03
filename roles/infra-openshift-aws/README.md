# infra-openshift-aws

This Ansible role manages the infrastructure configuration for an OpenShift cluster on AWS.

## Purpose

This role is designed to:
- Manage AWS infrastructure for OpenShift clusters
- Configure cluster infrastructure components
- Handle infrastructure lifecycle management

## Prerequisites

- An operational OpenShift cluster on AWS
- AWS credentials with appropriate permissions
- Cluster admin privileges

## Tasks

This role performs the following tasks:
- Adds ARM-based worker nodes to the cluster
- Applies appropriate labels to worker nodes
- Configures Let's Encrypt certificate for the cluster
- Sets up cluster authentication


## References

- [OpenShift on AWS Documentation](https://docs.openshift.com/container-platform/latest/installing/installing_aws/installing-aws-default.html)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
