# Nutanix Images Role

This Ansible role manages Nutanix images in a Nutanix cluster.

## Role Variables

The following variables are required:

- `image_name`: Name of the image to create or update
- `cluster_name`: Name of the cluster where the image will be created
- `storage_container`: Name of the storage container where the image will be stored

The following variables are optional:

- `description`: Description of the image (default: empty string)
- `image_type`: Type of the image (default: 'DISK_IMAGE')
- `source_uri`: Source URI for the image (default: empty string)
- `architecture`: Architecture of the image (default: 'X86_64')

## Example Usage

```yaml
- name: Create/Update Image
  hosts: localhost
  vars_files:
    - "vars/{{ environment_key }}/{{ site_key }}/images/var_{{ image_name }}.yaml"
    - var_vault.yaml
  tasks:
    - name: Include image role
      ansible.builtin.import_role:
        name: ntnx_images
        tasks_from: create_update_image.yaml
```

## Dependencies

This role depends on the following helper tasks:
- `task_helpers/get_cluster_uuids.yaml`
- `task_helpers/get_storage_containers_uuids.yaml`
- `task_helpers/get_image_uuids.yaml`
