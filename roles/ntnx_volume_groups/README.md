# ntnx_volume_groups Role

This Ansible role manages Nutanix volume groups, providing comprehensive functionality to create, update, configure, and manage volume groups with disk management and VM attachments.

## Role Structure

```
roles/ntnx_volume_groups/
├── tasks/
│   ├── main.yml                 # Main tasks file
│   ├── create_update_vg.yaml    # Create or update volume group
│   ├── create_update_vg_disks.yaml # Create or update volume group disks
│   ├── attach_vms.yaml          # Attach VMs to volume group
│   └── detach_vms.yaml          # Detach VMs from volume group
├── vars/                        # Role variables
├── defaults/                    # Default variables
├── handlers/                    # Role handlers
├── files/                       # Role files
├── templates/                   # Role templates
└── README.md                    # This file
```

## Tasks

### create_update_vg.yaml
- Checks if volume group exists
- Creates new volume group if it doesn't exist
- Updates existing volume group configuration
- Handles sharing status and flash mode settings
- Manages cluster reference

### create_update_vg_disks.yaml
- Creates or updates disks within the volume group
- Manages disk size and storage container settings
- Handles disk attachment to the volume group

### attach_vms.yaml
- Attaches VMs to the volume group
- Handles VM UUID lookups and validation
- Manages VM-group relationships

### detach_vms.yaml
- Detaches VMs from the volume group
- Handles cleanup of VM-group relationships
- Validates detachment operations

## Variables

### Required Variables
- `vg_name`: Name of the volume group
- `cluster_name`: Name of the cluster
- `storage_container`: Name of the storage container
- `attach_vms`: List of VMs to attach to the volume group
- `detach_vms`: List of VMs to detach from the volume group

### Optional Variables
- `sharing_status`: Volume group sharing status (default: "NONE")
- `enable_flash_mode`: Enable flash mode (default: false)
- `disk_size_gb`: Size of disks in GB
- `disk_count`: Number of disks to create

## Usage

This role is typically used by the main `volume_groups.yaml` playbook:

```bash
ansible-playbook volume_groups.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e nutanix_host=10.10.3.10 \
  --vault-pass-file .vault-token \
  -e vg_name=my_volume_group
```

## Example Variable File

```yaml
---
vg_name: "my_volume_group"
cluster_name: "KSALAB-NX"
storage_container: "SelfServiceContainer"
sharing_status: "NONE"
enable_flash_mode: false
attach_vms:
  - "VM1"
  - "VM2"
detach_vms:
  - "VM3"
disk_size_gb: 100
disk_count: 2
```

## Volume Group Features

### Data Management
- **Disk Creation**: Automated disk creation with configurable sizes
- **Storage Container Integration**: Proper storage container association
- **Flash Mode**: Optional flash mode for performance optimization

### VM Management
- **VM Attachments**: Automated VM attachment to volume groups
- **VM Detachments**: Clean VM detachment with validation
- **Relationship Management**: Proper VM-group relationship handling

### Configuration
- **Sharing Status**: Configurable sharing policies
- **Cluster Association**: Proper cluster reference management
- **Error Handling**: Comprehensive error handling and validation

## Dependencies

This role depends on the following helper tasks:
- `task_helpers/get_cluster_uuids.yaml`
- `task_helpers/get_storage_containers_uuids.yaml`
- `task_helpers/get_vm_uuids.yaml`
- `task_helpers/get_vg_uuids.yaml`

## License

BSD

## Author Information

Nutanix IaC Team
