# rhadp-bootstrap

## references

#### Installation
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/installing_on_aws/index
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/postinstallation_configuration/index

#### Other
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/security_and_compliance/cert-manager-operator-for-red-hat-openshift

## what needs to be done

- [ ] create a bastion host
    - [ ] install openshift-client
    - [ ] install kubectl
    - [ ] install oc
    - [ ] install awscli
    - [ ] install python3, python3-pip, ansible 
    - [ ] clone the repo into the bastion host
    - [ ] aws credentials, pull secret, aws
    - [ ] create a a certificate if none is provided

- [X] bootstrap a basic cluster
    - [X] Arm controll plane
    - [X] x86 compute nodes

- [ ] configure the cluster
    - [ ] add additional machine sets
        - [X] ARM compute nodes
        - [ ] GPU compute nodes (optional)
    - [X] add labels/taints to the machinesets
    - [X] install Let's Encrypt certificate
        - [X] install cert-manager operator
        - [X] create issuer and get a certificate
        - [X] configure ingress controller
        - [X] configure the cluster to use the certificate by default on all routes
    - [ ] configure OAuth and SSO
        - [ ] add Keycloak to the cluster
        - [ ] configure htpasswd provider and add default users
        - [ ] configure GitHub as identity provider
