# ntnx_subnets Role

This Ansible role manages Nutanix subnets, providing functionality to create, update, and configure subnets in a Nutanix cluster.

## Role Structure

```
roles/ntnx_subnets/
├── tasks/
│   ├── main.yml                 # Main tasks file
│   ├── create_update_subnet.yaml # Create or update subnet
│   └── configure_subnet.yaml    # Configure subnet settings
├── vars/                        # Role variables
├── defaults/                    # Default variables
├── handlers/                    # Role handlers
├── files/                       # Role files
├── templates/                   # Role templates
└── README.md                    # This file
```

## Tasks

### create_update_subnet.yaml
- Checks if subnet exists
- Creates new subnet if it doesn't exist
- Updates existing subnet configuration
- Handles validation and error conditions

### configure_subnet.yaml
- Configures DHCP settings for the subnet
- Sets up IP pool configurations
- Manages additional subnet parameters

## Variables

### Required Variables
- `subnet_name`: Name of the subnet
- `cluster_name`: Name of the cluster
- `subnet_type`: Type of subnet (VLAN, OVERLAY, etc.)
- `network_id`: Network identifier

## Usage

This role is typically used by the main `subnets.yaml` playbook:

```bash
ansible-playbook subnets.yaml \
  -e environment_key=nonprod \
  -e site_key=qd5 \
  -e nutanix_host=10.10.3.10 \
  --vault-pass-file .vault-token \
  -e subnet_name=VLAN-105
```

## Example Variable File

```yaml
---
name: VLAN-105
cluster_name: KSALAB-NX
subnet_type: VLAN
network_id: "105"
```
