# Variables Directory

This directory contains environment-specific variables and configurations for the Nutanix IaC project. Variables are organized by environment and site to provide clear separation between different deployment targets.

## 📁 Directory Structure

```
vars/
├── nonprod/                      # Non-production environments
│   ├── qd5/                     # QD5 site (non-production)
│   │   ├── var_auth.yaml        # Authentication variables
│   │   ├── var_subnet.yaml      # Subnet configuration
│   │   ├── var_categories.yaml  # Category definitions
│   │   ├── storagecontainers/   # Storage container variables
│   │   ├── volumegroups/        # Volume group variables
│   │   ├── images/              # Image variables
│   │   └── protectionpolicies/  # Protection policy variables
│   └── mv5/                     # MV5 site (non-production)
└── prod/                        # Production environments
    ├── qd5/                     # QD5 site (production)
    └── mv5/                     # MV5 site (production)
```

## 🏗️ Environment Organization

### Environment Levels
- **`nonprod/`**: Non-production environments for testing and development
- **`prod/`**: Production environments for live workloads

### Site Organization
- **`qd5/`**: QD5 site configurations
- **`mv5/`**: MV5 site configurations

## 📄 Variable File Types

### Global Configuration Files

#### `var_auth.yaml`
Contains authentication and connection variables for the Nutanix cluster.

**Example:**
```yaml
---
nutanix_username: "admin"
nutanix_password: "{{ vault_nutanix_password }}"
cluster_name: "KSALAB-NX"
```

#### `var_subnet.yaml`
Contains global subnet configuration and network settings.

**Example:**
```yaml
---
subnet_name: "VLAN-105"
cluster_name: "KSALAB-NX"
subnet_type: "VLAN"
network_id: "105"
```

#### `var_categories.yaml`
Contains category definitions and values for resource organization.

**Example:**
```yaml
---
categories:
  Environment:
    - Production
    - Development
    - Testing
  Application:
    - Web
    - Database
    - Backup
```

### Component-Specific Variables

#### Storage Containers (`storagecontainers/`)
- **File Pattern**: `var_{container_name}.yaml`
- **Purpose**: Storage container configuration and settings
- **Example**: `var_NewContainer.yaml`

#### Volume Groups (`volumegroups/`)
- **File Pattern**: `var_{vg_name}.yaml`
- **Purpose**: Volume group configuration, disk settings, and VM attachments
- **Example**: `var_gfgfg22July.yaml`

#### Images (`images/`)
- **File Pattern**: `var_{image_name}.yaml`
- **Purpose**: Image configuration, type, and source settings
- **Example**: `var_example_image.yaml`

#### Protection Policies (`protectionpolicies/`)
- **File Pattern**: `{scenario}.yaml`
- **Purpose**: Protection policy configurations with schedules and retention policies
- **Example**: `sc1.yaml`, `sc2.yaml`, etc.

## 🔐 Security and Vault

### Sensitive Data Handling
- **Vault Encryption**: Sensitive data is encrypted using Ansible Vault
- **Password Management**: Passwords and credentials are stored in `var_vault.yaml`
- **Access Control**: Environment-specific access controls

### Vault File Structure
```yaml
# var_vault.yaml (encrypted)
---
nutanix_password: "encrypted_password"
api_keys: "encrypted_api_keys"
```

## 📝 Variable File Templates

### Storage Container Template
```yaml
---
name: "ContainerName"
cluster_name: "{{ cluster_name }}"
replication_factor: 2
compression_enabled: true
deduplication_enabled: "NONE"
```

### Volume Group Template
```yaml
---
vg_name: "VolumeGroupName"
cluster_name: "{{ cluster_name }}"
storage_container: "ContainerName"
sharing_status: "NONE"
enable_flash_mode: false
attach_vms:
  - "VM1"
  - "VM2"
detach_vms: []
disk_size_gb: 100
disk_count: 2
```

### Image Template
```yaml
---
image_name: "ImageName"
description: "Image description"
cluster_name: "{{ cluster_name }}"
storage_container: "ContainerName"
image_type: "DISK_IMAGE"
source_uri: ""
architecture: "X86_64"
```

### Protection Policy Template
```yaml
---
protection_policies:
  policy_name:
    schedules:
      - source_availability_zone_url: "az_name"
        destinations_availability_zone_url: "destination_az_name"
schedules:
  - snapshot_type: "CRASH_CONSISTENT"
    rpo: 1
    rpo_unit: "MINUTE"
    local_retention_policy:
      num_snapshots: 30
```

## 🚀 Usage Patterns

### Environment-Specific Execution
```bash
# Non-production
ansible-playbook playbook.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  --vault-pass-file .vault-token

# Production
ansible-playbook playbook.yaml \
  -e environment_key=prod \
  -e site_key=qd5 \
  --vault-pass-file .vault-token
```

### Component-Specific Execution
```bash
# Storage Container
ansible-playbook storage_containers.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e container_name=MyContainer

# Volume Group
ansible-playbook volume_groups.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e vg_name=MyVolumeGroup

# Image
ansible-playbook images.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e image_name=MyImage

# Protection Policy
ansible-playbook protection_policies.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e scenario=sc1
```

## 🔧 Variable Precedence

1. **Command Line Variables** (`-e` parameters)
2. **Component-Specific Variables** (`var_{component}.yaml`)
3. **Global Variables** (`var_auth.yaml`, `var_subnet.yaml`, etc.)
4. **Vault Variables** (`var_vault.yaml`)
5. **Role Defaults** (in `roles/{role}/defaults/main.yml`)

## 📋 Best Practices

### Variable Organization
1. **Environment Separation**: Keep production and non-production variables separate
2. **Site Isolation**: Organize by site for multi-site deployments
3. **Component Grouping**: Group related variables by infrastructure component
4. **Naming Conventions**: Use consistent naming patterns

### Security Guidelines
1. **Vault Encryption**: Encrypt all sensitive data
2. **Access Control**: Limit access to production variables
3. **Audit Trail**: Maintain change logs for variable modifications
4. **Backup**: Regular backups of variable files

### Maintenance
1. **Documentation**: Keep variable files well-documented
2. **Validation**: Validate variable syntax and structure
3. **Testing**: Test variable changes in non-production first
4. **Version Control**: Use version control for all variable files

## 🔍 Troubleshooting

### Common Issues
1. **Variable Not Found**: Check file paths and naming conventions
2. **Vault Decryption**: Verify vault password and file permissions
3. **Environment Mismatch**: Ensure correct environment and site keys
4. **Syntax Errors**: Validate YAML syntax in variable files

### Debugging
- Use `ansible-playbook` with `-vvv` for verbose variable output
- Check variable precedence with `ansible-playbook --list-tasks`
- Validate variable files with YAML linters

## 📚 Related Documentation

- [Project README](../README.md) for overall project structure
- [Task Helpers README](../task_helpers/README.md) for helper task documentation
- [Individual Role READMEs](../roles/) for role-specific variable requirements 