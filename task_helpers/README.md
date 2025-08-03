# Task Helpers

This directory contains reusable Ansible tasks for common operations in the Nutanix IaC project. These helper tasks are designed to be included in playbooks and roles to provide standardized functionality for UUID lookups and common infrastructure operations.

## 📋 Available Helper Tasks

### UUID Lookup Tasks

#### `get_cluster_uuids.yaml`
Fetches cluster UUIDs by name and creates a mapping dictionary.

**Variables:**
- `ntnx_cluster_list`: List of cluster names to look up

**Output:**
- `cluster_name_to_uuid`: Dictionary mapping cluster names to UUIDs

**Usage:**
```yaml
- name: Fetch cluster uuids
  ansible.builtin.include_tasks: task_helpers/get_cluster_uuids.yaml
  vars:
    ntnx_cluster_list:
      - "{{ cluster_name }}"
```

#### `get_subnet_uuids.yaml`
Fetches subnet UUIDs by name and creates a mapping dictionary.

**Variables:**
- `ntnx_subnets_list`: List of subnet names to look up

**Output:**
- `subnets_name_to_uuid`: Dictionary mapping subnet names to UUIDs

**Usage:**
```yaml
- name: Fetch subnet uuids
  ansible.builtin.include_tasks: task_helpers/get_subnet_uuids.yaml
  vars:
    ntnx_subnets_list:
      - "{{ subnet_name }}"
```

#### `get_storage_containers_uuids.yaml`
Fetches storage container UUIDs by name and creates a mapping dictionary.

**Variables:**
- `ntnx_storage_containers_list`: List of storage container names to look up

**Output:**
- `storage_containers_name_to_uuid`: Dictionary mapping storage container names to UUIDs

**Usage:**
```yaml
- name: Fetch storage container IDs
  ansible.builtin.include_tasks: task_helpers/get_storage_containers_uuids.yaml
  vars:
    ntnx_storage_containers_list:
      - "{{ storage_container }}"
```

#### `get_vm_uuids.yaml`
Fetches VM UUIDs by name and creates a mapping dictionary.

**Variables:**
- `ntnx_vms_list`: List of VM names to look up

**Output:**
- `vms_name_to_uuid`: Dictionary mapping VM names to UUIDs

**Usage:**
```yaml
- name: Get list of VMs and their IDs
  ansible.builtin.include_tasks: task_helpers/get_vm_uuids.yaml
  vars:
    ntnx_vms_list: "{{ attach_vms + detach_vms }}"
```

#### `get_vg_uuids.yaml`
Fetches volume group UUIDs by name and creates a mapping dictionary.

**Variables:**
- `ntnx_vgs_list`: List of volume group names to look up

**Output:**
- `vgs_name_to_uuid`: Dictionary mapping volume group names to UUIDs

**Usage:**
```yaml
- name: Get VG and their IDs
  ansible.builtin.include_tasks: task_helpers/get_vg_uuids.yaml
  vars:
    ntnx_vgs_list:
      - "{{ vg_name }}"
```

#### `get_image_uuids.yaml`
Fetches image UUIDs by name and creates a mapping dictionary.

**Variables:**
- `ntnx_images_list`: List of image names to look up

**Output:**
- `images_name_to_uuid`: Dictionary mapping image names to UUIDs

**Usage:**
```yaml
- name: Get Image and their IDs
  ansible.builtin.include_tasks: task_helpers/get_image_uuids.yaml
  vars:
    ntnx_images_list:
      - "{{ image_name }}"
```

#### `get_protection_policy_uuids.yaml`
Fetches protection policy UUIDs by name and creates a mapping dictionary.

**Variables:**
- `ntnx_protection_policies_list`: List of protection policy names to look up

**Output:**
- `protection_policies_name_to_uuid`: Dictionary mapping protection policy names to UUIDs

**Usage:**
```yaml
- name: Get Protection Policies and their IDs
  ansible.builtin.include_tasks: task_helpers/get_protection_policy_uuids.yaml
  vars:
    ntnx_protection_policies_list: "{{ protection_policies.keys() | list }}"
```

#### `get_availability_zone_uuids.yaml`
Fetches availability zone UUIDs by name and creates a mapping dictionary.

**Variables:**
- `ntnx_availability_zones_list`: List of availability zone names to look up

**Output:**
- `availability_zones_name_to_uuid`: Dictionary mapping availability zone names to UUIDs

**Usage:**
```yaml
- name: Fetch availability zone IDs
  ansible.builtin.include_tasks: task_helpers/get_availability_zone_uuids.yaml
  vars:
    ntnx_availability_zones_list: "{{ availability_zones_list | default([]) }}"
```

### Additional Helper Tasks

#### `get_subnet.yaml`
Provides additional subnet information and validation.

**Variables:**
- `subnet_name`: Name of the subnet to look up

**Output:**
- `subnet_info`: Detailed subnet information

**Usage:**
```yaml
- name: Get subnet information
  ansible.builtin.include_tasks: task_helpers/get_subnet.yaml
  vars:
    subnet_name: "{{ subnet_name }}"
```

## 🔧 Common Patterns

### UUID Mapping Pattern
All UUID lookup tasks follow a consistent pattern:

1. **Fetch Information**: Use the appropriate Nutanix module to fetch entity information
2. **Filter by Name**: Apply name-based filtering to get specific entities
3. **Create Mapping**: Build a dictionary mapping names to UUIDs
4. **Handle Empty Results**: Gracefully handle cases where entities don't exist

### Error Handling
All helper tasks include proper error handling:
- Check for empty response arrays
- Validate UUID existence before mapping
- Provide meaningful error messages

### Performance Optimization
- Use `select: "extId"` to minimize data transfer
- Apply filters at the API level
- Cache results in variables for reuse

## 📝 Usage Guidelines

### When to Use Helper Tasks
- **UUID Lookups**: Always use helper tasks for UUID lookups instead of hardcoding
- **Reusable Operations**: Use for operations that are repeated across multiple playbooks
- **Standardization**: Ensure consistent behavior across the project

### Best Practices
1. **Variable Naming**: Use consistent variable naming conventions
2. **Error Handling**: Always check for empty results
3. **Documentation**: Document any custom variables or outputs
4. **Testing**: Test helper tasks with various input scenarios

### Integration with Roles
Helper tasks are designed to work seamlessly with roles:
- Provide required UUID mappings
- Handle common validation scenarios
- Support role-specific requirements

## 🔍 Troubleshooting

### Common Issues
1. **Empty UUID Mappings**: Check if the entity names exist in the cluster
2. **Permission Errors**: Verify Nutanix API access permissions
3. **Network Issues**: Ensure connectivity to the Nutanix cluster

### Debugging
- Use `ansible-playbook` with `-vvv` for verbose output
- Check the `register` variables for detailed API responses
- Validate input variables before including helper tasks

## 📚 Related Documentation

- [Nutanix Ansible Collection Documentation](https://docs.ansible.com/ansible/latest/collections/nutanix/ncp/)
- [Ansible Task Documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_tasks.html)
- [Project README](../README.md) for overall project structure 