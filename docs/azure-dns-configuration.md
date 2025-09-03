# Azure DNS Configuration for VM Instances

## Overview

The VM creation process now includes automatic DNS record creation for Azure deployments. This creates stable DNS entries that persist through VM restarts since they're tied to static public IP addresses.

## Required Variables

To enable DNS record creation, add the following variables to your inventory file:

### Required DNS Variables

```yaml
# Azure DNS Zone Configuration
azure_dns_zone_name: "example.com"  # The DNS zone name where records will be created
```

### Optional DNS Variables

```yaml
# Optional: If DNS zone is in a different resource group than the VM
azure_dns_resource_group: "my-dns-resource-group"  # Defaults to azure_resource_group if not specified

# Optional: Custom DNS name for the instance
instance_dns_name: "my-custom-name"  # Defaults to instance_name if not specified
```

## Configuration Example

Add to your `inventory/azure-compact.yml` or similar inventory file:

```yaml
all:
  vars:
    # ... existing variables ...
    
    # DNS Configuration
    azure_dns_zone_name: "redhatworkshops.io"
    # azure_dns_resource_group: "dns-resource-group"  # Optional, uses azure_resource_group if not set
    # instance_dns_name: "builder-01"  # Optional, uses instance_name if not set
```

## What Gets Created

When `azure_dns_zone_name` is defined, the VM creation process will:

1. Create a static public IP address (existing functionality)
2. Create an A record in the specified DNS zone pointing to the public IP
3. Set up facts for both the IP address and DNS FQDN
4. Configure SSH connections to use the DNS name when available

## Prerequisites

1. **Azure DNS Zone**: The DNS zone specified in `azure_dns_zone_name` must already exist in Azure
2. **Permissions**: The Azure service principal must have DNS Zone Contributor permissions on the DNS zone
3. **Resource Group**: The DNS zone must be accessible from the same subscription

## DNS Record Details

- **Record Type**: A record
- **TTL**: 300 seconds (5 minutes)
- **Record Name**: `{{ instance_dns_name | default(instance_name) }}`
- **Value**: The static public IP address of the VM

## Example Result

If you have:
- `instance_name: "builder-vm"`
- `azure_dns_zone_name: "example.com"`

The following will be created:
- DNS A record: `builder-vm.example.com` → `20.1.2.3` (example IP)
- Available fact: `instance_dns_fqdn` = `"builder-vm.example.com"`

## Troubleshooting

### Common Issues

1. **DNS Zone Not Found**
   - Ensure the DNS zone exists in Azure
   - Verify the zone name is correct
   - Check that the service principal has access to the DNS resource group

2. **Permission Denied**
   - Ensure the Azure service principal has "DNS Zone Contributor" role
   - Verify the subscription ID is correct

3. **Resource Group Issues**
   - If DNS zone is in a different resource group, set `azure_dns_resource_group`
   - Ensure the specified resource group exists

### Validation

After deployment, you can verify the DNS record:

```bash
# Check DNS resolution
nslookup builder-vm.example.com

# Or using dig
dig builder-vm.example.com A
```

## Benefits

- **Stable DNS Names**: DNS names remain consistent across VM restarts
- **Easy Access**: Use meaningful hostnames instead of IP addresses
- **SSL/TLS Friendly**: Proper DNS names make certificate management easier
- **Service Discovery**: Other services can reliably connect using DNS names
