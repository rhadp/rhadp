## infra-openshift-aws

This role manages the infrastructure for an OpenShift cluster on AWS.

The following mofifications are made:

- add ARM worker nodes
- apply the correct labels to the worker nodes
- configure the cluster to use a Let's Encrypt certificate
- configure authentication