# rhadp-bootstrap

## references
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/installing_on_aws/index
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/postinstallation_configuration/index

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
    - [ ] apply correct labels to the nodes
    - [ ] install Let's Encrypt certificate
    - [ ] configure OAuth and SSO
        - [ ] add Keycloak to the cluster
        - [ ] configure htpasswd provider and add default users
        - [ ] configure GitHub as identity provider
