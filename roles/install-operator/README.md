# install-operator

This role installs an operator from the Red Hat Operators catalog.

## Parameters

- `operator_subscription_name`: The name of the subscription to install.
- `operator_deployment_name`: The name of the deployment to wait for.
- `operator_namespace`: The namespace to install the operator in.
- `wait_for_operator`: Whether to wait for the operator to be ready.
- `create_operator_namespace`: Whether to create the operator namespace if it doesn't exist.
- `create_operator_group`: Whether to create an OperatorGroup for the operator.
- `manage_all_namespaces`: Whether to configure the operator to manage resources across all namespaces.
- `subscription_channel`: The channel to subscribe to.
- `subscription_starting_csv`: The starting CSV to subscribe to.
- `install_plan_approval`: The approval mode for the subscription.
- `source`: The source of the subscription.
- `source_namespace`: The namespace of the source.

## Default values

```yaml
# installation defaults
operator_namespace: "openshift-operators"
wait_for_operator: true
create_operator_namespace: true
create_operator_group: false
manage_all_namespaces: true

# subscription defaults
subscription_channel: "latest"
subscription_starting_csv: ""
install_plan_approval: "Automatic"
source: "redhat-operators"
source_namespace: "openshift-marketplace"
```

## Usage

```yaml
- name: install some operator
  include_role:
    name: install-operator
  vars:
    operator_subscription_name: "my-operator"
    operator_deployment_name: "my-operator"
    operator_namespace: "my-namespace"
    wait_for_operator: true
    subscription_channel: "latest"
    subscription_starting_csv: ""
    install_plan_approval: "Automatic"
    source: "redhat-operators"
    source_namespace: "openshift-marketplace"
```
