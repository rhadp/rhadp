# create_vm_azure

This Ansible role creates a RHEL 10 VM on Azure using ARM64 architecture.

## Features

- Creates a RHEL 10 VM with ARM64 architecture (Standard_D2ps_v5)
- Sets up all necessary Azure networking resources (Resource Group, VNet, Subnet, NSG)
- Configures security group rules for SSH, HTTP, and HTTPS access
- Attaches and mounts an additional data disk
- Supports both installation and cleanup operations

## Architecture

The role creates the following Azure resources:
1. Resource Group (uses existing `azure_resourcegroup` or creates new with azure_guid)
2. Virtual Network and Subnet (named with azure_guid suffix)
3. Network Security Group with firewall rules (SSH, HTTP, HTTPS)
4. Public IP Address and Network Interface
5. Virtual Machine with OS and data disks

The data disk is automatically formatted with ext4 and mounted at `/mnt/data` with persistent fstab entry.