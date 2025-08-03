# Nutanix Infrastructure as Code (IaC)

This repository contains Ansible playbooks and roles for managing Nutanix infrastructure components including subnets, storage containers, volume groups and images.

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Playbooks](#playbooks)
- [Roles](#roles)
- [Variables](#variables)
- [Usage Examples](#usage-examples)
- [Helper Tasks](#helper-tasks)
- [Contributing](#contributing)

## 🎯 Overview

This project provides a comprehensive set of Ansible playbooks and roles for managing Nutanix infrastructure components. It follows a modular approach with separate roles for each infrastructure component and environment-specific variable files.

## 🔧 Prerequisites

- Ansible 2.9 or higher
- Nutanix Prism Central access
- Nutanix Ansible Collection (`nutanix.ncp`)
- Access to the target Nutanix cluster
- Vault password file (`.vault-token`)

## 📁 Project Structure

```
nutanix-iaac/
├── README.md                      # This file
├── .vault-token                   # Vault password file
├── var_vault.yaml                 # Encrypted vault variables
├── task_helpers/                  # Helper tasks for UUID lookups
├── roles/                         # Ansible roles
├── vars/                          # Environment-specific variables
│   ├── nonprod/                   # Non-production environments
│   └── prod/                      # Production environments
└── Playbooks:
    ├── categories.yaml            # Category management
    ├── subnets.yaml              # Subnet creation/update
    ├── delete_subnets.yaml       # Subnet deletion
    ├── storage_containers.yaml   # Storage container creation/update
    ├── delete_storage_containers.yaml # Storage container deletion
    ├── volume_groups.yaml        # Volume group creation/update
    ├── delete_volume_groups.yaml # Volume group deletion
    ├── images.yaml               # Image creation/update
    ├── delete_images.yaml        # Image deletion
```

## 🎮 Playbooks

### Categories Management
- **`categories.yaml`**: Create and manage Nutanix categories and category values
- **Usage**: `ansible-playbook categories.yaml -e environment_key=nonprod -e site_key=qd5 -e nutanix_host=10.10.3.10 --vault-pass-file .vault-token`

### Subnet Management
- **`subnets.yaml`**: Create and update subnets in Nutanix
- **`delete_subnets.yaml`**: Delete existing subnets
- **Usage**: `ansible-playbook subnets.yaml -e environment_key=nonprod -e site_key=qd5 -e nutanix_host=10.10.3.10 --vault-pass-file .vault-token -e cluster_name=KSALAB-NX -e subnet_name=VLAN-105`

### Storage Container Management
- **`storage_containers.yaml`**: Create and update storage containers
- **`delete_storage_containers.yaml`**: Delete storage containers
- **Usage**: `ansible-playbook storage_containers.yaml -e environment_key=nonprod -e site_key=qd5 -e nutanix_host=10.10.3.10 --vault-pass-file .vault-token -e container_name=NewContainer`

### Volume Group Management
- **`volume_groups.yaml`**: Create and update volume groups with disk management and VM attachments
- **`delete_volume_groups.yaml`**: Delete volume groups and detach VMs
- **Usage**: `ansible-playbook volume_groups.yaml -e environment_key=nonprod -e site_key=qd5 -e nutanix_host=10.10.3.10 --vault-pass-file .vault-token  -e vg_name=gfgfg22July`

### Image Management
- **`images.yaml`**: Create and update Nutanix images
- **`delete_images.yaml`**: Delete existing images
- **Usage**: `ansible-playbook images.yaml -e environment_key=nonprod -e site_key=qd5 -e nutanix_host=10.10.3.10 --vault-pass-file .vault-token -e image_name=my_image`

## 🎭 Roles

### ntnx_subnets
Manages Nutanix subnets with IP address management and network configuration.

### ntnx_storage_containers
Handles storage container creation, updates, and deletion with compression and erasure coding settings.

### ntnx_volume_groups
Comprehensive volume group management including disk creation, VM attachments, and flash mode configuration.

### ntnx_images
Manages Nutanix images with support for different image types, architectures, and source URIs.

## 📊 Variables

### Environment Structure
Variables are organized by environment and site:
- `vars/{environment_key}/{site_key}/`
  - `var_auth.yaml` - Authentication variables
  - `var_subnet.yaml` - Subnet configuration
  - `var_categories.yaml` - Category definitions
  - `{component}/` - Component-specific variables

### Component Variable Files
- **Subnets**: `vars/{env}/{site}/var_{subnet_name}.yaml`
- **Storage Containers**: `vars/{env}/{site}/storagecontainers/var_{container_name}.yaml`
- **Volume Groups**: `vars/{env}/{site}/volumegroups/var_{vg_name}.yaml`
- **Images**: `vars/{env}/{site}/images/var_{image_name}.yaml`

## 🚀 Usage Examples

### Basic Playbook Execution
```bash
# Set environment variables
export ENVIRONMENT_KEY=nonprod
export SITE_KEY=qd5
export NUTANIX_HOST=10.10.3.10

# Execute playbook
ansible-playbook {playbook_name}.yaml \
  -e environment_key=$ENVIRONMENT_KEY \
  -e site_key=$SITE_KEY \
  -e nutanix_host=$NUTANIX_HOST \
  --vault-pass-file .vault-token \
  -e {component_name}=my_component
```

### Common Operations

#### Create a Subnet
```bash
ansible-playbook subnets.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e nutanix_host=10.10.3.10 \
  --vault-pass-file .vault-token \
  -e subnet_name=my_subnet
```

#### Create a Volume Group with VMs
```bash
ansible-playbook volume_groups.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e nutanix_host=10.10.3.10 \
  --vault-pass-file .vault-token \
  -e vg_name=my_volume_group
```

## 🔧 Helper Tasks

The `task_helpers/` directory contains reusable tasks for:
- **UUID Lookups**: Get UUIDs for clusters, subnets, storage containers, VMs, volume groups, images, and availability zones
- **Common Operations**: Standardized tasks for common infrastructure operations

### Available Helper Tasks
- `get_cluster_uuids.yaml`
- `get_subnet_uuids.yaml`
- `get_storage_containers_uuids.yaml`
- `get_vm_uuids.yaml`
- `get_vg_uuids.yaml`
- `get_image_uuids.yaml`
- `get_availability_zone_uuids.yaml`

## 🔐 Security

- **Vault Encryption**: Sensitive data is encrypted using Ansible Vault
- **Environment Separation**: Clear separation between production and non-production environments
- **Access Control**: Role-based access control through Nutanix Prism Central

## 📝 Contributing

1. Follow the existing code structure and naming conventions
2. Add appropriate documentation for new roles and playbooks
3. Include example variable files for new components
4. Test playbooks in non-production environments first
5. Update this README.md when adding new functionality

## 📞 Support

For issues and questions:
1. Check the individual role README files for specific documentation
2. Review the example variable files in the `vars/` directory
3. Consult the Nutanix Ansible Collection documentation
4. Contact the Nutanix IaC team
