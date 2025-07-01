# infra-openshift-aws

This Ansible role manages the infrastructure configuration for an OpenShift cluster on AWS.

This role performs the following tasks:
- Adds ARM-based worker nodes to the cluster
- Applies appropriate labels to worker nodes
- Configures Let's Encrypt certificate for the cluster

## References

- [OpenShift on AWS Documentation](https://docs.openshift.com/container-platform/latest/installing/installing_aws/installing-aws-default.html)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
