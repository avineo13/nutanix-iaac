# ntnx_storage_containers Role

This Ansible role manages Nutanix storage containers, providing functionality to create, update, and configure storage containers in a Nutanix cluster.

## Role Structure

```
roles/ntnx_storage_containers/
├── tasks/
│   ├── main.yml                 # Main tasks file
│   ├── create_update_container.yaml # Create or update storage container
│   └── configure_container.yaml # Configure storage container settings
├── vars/                        # Role variables
├── defaults/                    # Default variables
├── handlers/                    # Role handlers
├── files/                       # Role files
├── templates/                   # Role templates
└── README.md                    # This file
```

## Tasks

### create_update_container.yaml
- Checks if storage container exists
- Creates new storage container if it doesn't exist
- Updates existing storage container configuration
- Handles validation and error conditions

### configure_container.yaml
- Configures QoS settings for the storage container
- Sets up encryption settings
- Manages container tags and metadata

## Variables

### Required Variables
- `container_name`: Name of the storage container
- `cluster_name`: Name of the cluster

### Optional Variables
- `replication_factor`: Replication factor (default: 2)
- `erasure_code_profile`: Erasure code profile name
- `compression_enabled`: Enable compression (true/false)
- `deduplication_enabled`: Enable deduplication (NONE/OFF/POST_PROCESS)

## Usage

This role is typically used by the main `storage_containers.yaml` playbook:

```bash
ansible-playbook storage_containers.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e nutanix_host=10.10.3.10 \
  --vault-pass-file .vault-token \
  -e container_name=SelfServiceContainer
```

## Example Variable File

```yaml
---
name: SelfServiceContainer
cluster_name: KSALAB-NX
replication_factor: 2
compression_enabled: true
deduplication_enabled: NONE
```

## Storage Container Features

### Data Protection
- **Replication Factor**: Configurable replication for data protection
- **Erasure Coding**: Support for erasure code profiles
- **Encryption**: Optional encryption at rest

### Performance Optimization
- **Compression**: Configurable compression policies
- **Deduplication**: Configurable deduplication policies
- **QoS**: IOPS and bandwidth limits

### Management
- **Tags**: Custom tags for organization and management
- **Cluster Association**: Proper cluster reference management