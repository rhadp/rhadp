## install-openshift-aws

This role installs a basic OpenShift cluster on AWS.

This role does the following:

- create the basic cluster config file
- create the aws credentials file
- create the cluster
- wait for the cluster to be ready

### Prerequisites

- AWS credentials
- pull secret
- openshift installer
