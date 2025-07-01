# lifecycle-aws

This Ansible role manages the lifecycle of the OpenShift cluster EC2 instances.

This role is designed to:
- Handle cluster start/stop operations


## Required Variables

- `aws_access_key_id`: AWS access key ID
- `aws_secret_access_key`: AWS secret access key
- `aws_default_region`: AWS region where the cluster is deployed
- `cluster_name`: Name of the cluster (used to filter EC2 instances)
- `ACTION`: Action to perform (START/STOP)

## Optional Variables

- `wait_timeout`: Timeout for cluster readiness wait (default: 600 seconds)
- `instance_tags`: Additional tags to filter instances
- `verify_cluster`: Whether to verify cluster state (default: true)

## References

- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [OpenShift on AWS Documentation](https://docs.openshift.com/container-platform/latest/installing/installing_aws/installing-aws-default.html)

