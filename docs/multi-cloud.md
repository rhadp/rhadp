# Multi-Cloud Deployment

RHADP provides a unified deployment experience across AWS, Azure, and GCP.

## AWS Deployment

### Instance Types

| Node Type | Instance Type | vCPU | Memory | Architecture | Count |
|-----------|---------------|------|--------|--------------|-------|
| **Control Plane** | `m8g.2xlarge` | 8 | 32 GiB | ARM64 (Graviton4) | 3 |
| **Worker Nodes** | `m8g.xlarge` | 4 | 16 GiB | ARM64 (Graviton4) | 3 |
| **Bare-Metal** (optional) | `m6g.metal` | 64 | 256 GiB | ARM64 (Graviton2) | 1 |

**Override to x86:**
```yaml
aws_control_plane_instance_type: "m6a.2xlarge"
aws_control_plane_architecture: "amd64"
aws_worker_instance_type: "m6a.xlarge"
aws_worker_architecture: "amd64"
```

### Reference

- [Installing on AWS - OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html/installing_on_aws/index)
- [AWS Graviton Processors](https://aws.amazon.com/ec2/graviton/)

## Azure Deployment

### Instance Types

| Node Type | Instance Type | vCPU | Memory | Architecture | Count |
|-----------|---------------|------|--------|--------------|-------|
| **Control Plane** | `Standard_D8s_v5` | 8 | 32 GiB | x86_64 (AMD) | 3 |
| **Worker Nodes** | `Standard_D8s_v5` | 8 | 32 GiB | x86_64 (AMD) | 3 |
| **ARM Workers** (optional) | `Standard_D8ps_v5` | 8 | 32 GiB | ARM64 (Ampere) | Configurable |

### Reference

- [Installing on Azure - OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_azure/index)
- [Dpsv5 Series (ARM)](https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/general-purpose/dpsv5-series)

## GCP Deployment

### Instance Types

| Node Type | Instance Type | vCPU | Memory | Architecture | Count |
|-----------|---------------|------|--------|--------------|-------|
| **Control Plane** | `t2a-standard-8` | 8 | 32 GiB | ARM64 (Ampere Altra) | 3 |
| **Worker Nodes** | `t2a-standard-8` | 8 | 32 GiB | ARM64 (Ampere Altra) | 3 |

**Override to x86:**
```yaml
gcp_control_plane_instance_type: "n2-standard-8"
gcp_control_plane_architecture: "amd64"
```

### Reference

- [Installing on GCP - OpenShift 4.19](https://docs.redhat.com/en/documentation/openshift_container_platform/4.19/html-single/installing_on_gcp/index)
- [Tau T2A Machine Series (ARM)](https://cloud.google.com/compute/docs/general-purpose-machines#t2a_machines)
