# Multi-Architecture Support

RHADP supports x86_64, ARM64, and hybrid (multi-architecture) clusters. Additionally, bare-metal nodes can be added if needed.

## x86_64 Clusters

**Configuration:**
```yaml
cluster_arch: "x86_64"
```

**Characteristics:**
- Broadest ecosystem support for container images
- Maximum compatibility with third-party software
- Widely available across all cloud providers

**Instance Types:**
- **AWS**: `m6a.2xlarge` (control), `m6a.xlarge` (workers)
- **Azure**: `Standard_D8s_v5`
- **GCP**: `n2-standard-8`

## ARM64 (aarch64) Clusters

**Configuration:**
```yaml
cluster_arch: "aarch64"
```

**Characteristics:**
- **Cost Savings**: 10-20% lower cost vs x86_64
- **Power Efficiency**: Better performance-per-watt
- **Automotive Alignment**: ARM common in automotive ECUs
- Growing ecosystem for ARM64 container images

**Instance Types:**
- **AWS**: `m8g.2xlarge` (control), `m8g.xlarge` (workers), `m6g.metal` (bare-metal)
- **Azure**: `Standard_D8ps_v5` (Ampere Altra)
- **GCP**: `t2a-standard-8` (Ampere Altra)

**Considerations:**
- Verify container images support ARM64
- Some legacy software may not have ARM builds
- Cross-compilation may be required

## Hybrid (Multi-Architecture) Clusters

**Platform Support**: AWS and Azure only (GCP not supported for multi-arch)

**Configuration:**
```yaml
cluster_arch: "multi"
control_plane_arch: "amd64"  # or "arm64"
setup_arm_worker_nodes: true
```

**Characteristics:**
- Run x86 and ARM workloads in same cluster
- Kubernetes scheduler automatically places pods on compatible nodes
- Test ARM compatibility while maintaining x86 fallback

### Architecture Patterns

**Pattern 1: x86 Control Plane + ARM Workers**
```yaml
cluster_arch: "multi"
control_plane_arch: "amd64"
setup_arm_worker_nodes: true
```

OTHER PATTERNS: TBD
