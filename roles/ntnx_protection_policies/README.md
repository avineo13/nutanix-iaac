# Nutanix Protection Policies Role

This Ansible role manages Nutanix protection policies in a Nutanix cluster.

## Role Variables

The following variables are required:

- `protection_policies`: Dictionary containing protection policy configurations
- `cluster_name`: Name of the cluster where the protection policies will be created
- `availability_zones_list`: List of availability zone names (optional)

## Protection Policy Structure

Each protection policy should have the following structure:

```yaml
protection_policies:
  policy_name:
    schedules:
      - source_availability_zone_url: "az_name"
        destinations_availability_zone_url: "destination_az_name"
      # ... more schedules
```

## Schedule Configuration

Schedules can include:
- `snapshot_type`: Type of snapshot (e.g., "CRASH_CONSISTENT")
- `rpo`: Recovery Point Objective value
- `rpo_unit`: Unit for RPO (e.g., "MINUTE", "HOUR", "DAY")
- `local_retention_policy`: Local retention configuration
- `remote_snapshot_retention_policy`: Remote retention configuration

## Example Usage

```yaml
- name: Create/Update Protection Policies
  hosts: localhost
  vars_files:
    - "vars/{{ environment_key }}/{{ site_key }}/protectionpolicies/{{ scenario }}.yaml"
    - var_vault.yaml
  tasks:
    - name: Include protection policies role
      ansible.builtin.import_role:
        name: ntnx_protection_policies
        tasks_from: create_update_protection_policies.yaml
```

## Dependencies

This role depends on the following helper tasks:
- `task_helpers/get_cluster_uuids.yaml`
- `task_helpers/get_availability_zone_uuids.yaml`
- `task_helpers/get_protection_policy_uuids.yaml` 