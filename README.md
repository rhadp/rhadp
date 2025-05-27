# rhadp-bootstrap

## references
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/installing_on_aws/index

## what needs to be done

- [X] create a a certificate if none is provided
- [X] bootstrap a basic cluster
- [ ] configure the cluster
    - [ ] add additional machine sets
        - [X] x86 set for general purpose & infra
        - [ ] GPU set for AI (optional)
    - [ ] apply correct labels to the nodes
    - [ ] install Let's Encrypt certificate
    - [ ] configure OAuth and SSO
        - [ ] add Keycloak to the cluster
        - [ ] configure htpasswd provider and add default users
        - [ ] configure GitHub as identity provider
    - [ ] 