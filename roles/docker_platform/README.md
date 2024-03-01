molecule.docker_platform
=========

Create a docker-based test platform for Molecule.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

```yaml
# Name of this Molecule platform
docker_platform_name: instance

# Whether this platform should be deployed on the current system (present/absent)
docker_platform_state: present

# Docker image that this platform runs
docker_platform_image: "geerlingguy/docker-rockylinux9-ansible:latest"

# Should the provided image be modified at runtime
docker_platform_modify_image: false

# Path to docker build files that should be used to modify the image. Files are treated as templates
#  and can contain jinja2 templating language ("{{ my_var }}" etc.)
docker_platform_modify_image_buildpath: "{{ molecule_ephemeral_directory }}/build"

# Command to be executed at runtime on the container
#  Leave as "" to use container default
docker_platform_command: ""

# Is this a SystemD enabled container?
docker_platform_systemd: true

# A list of Docker volumes that should be attached to the container
docker_platform_volumes: []

# Run the container in Privileged mode (greater host access, less security!)
docker_platform_privileged: false

# A list of tmpfs filesystem paths to be passed to the container
docker_platform_tmpfs: []
```

Configuration that should not require modification:  
```yaml
# Filesystem location of the Molecule ephemeral directory. Should not need to be updated by the user of this role!
docker_platform_molecule_ephemeral_directory: "{{ molecule_ephemeral_directory }}"
```

Molecule variables expected:
- `molecule_ephemeral_directory`

Dependencies
------------

TBA

Example Playbook
----------------

```yaml
- name: Create
  hosts: localhost
  gather_facts: false
  tasks:
    - name: Create platform
      ansible.builtin.include_role:
        name: syndr.molecule.docker_platform
      vars:
        docker_platform_name: "{{ item.name }}"
        docker_platform_image: "{{ item.image }}"
        docker_platform_systemd: true
      loop: "{{ molecule_yml.platforms }}"
      loop_control:
        label: item.name

# we want to avoid errors like "Failed to create temporary directory"
- name: Validate molecule inventory
  hosts: molecule
  gather_facts: false
  tasks:
    - name: Check kernel version
      ansible.builtin.raw: uname -a
      register: result
      changed_when: false

    - name: Display kernel info
      ansible.builtin.debug:
        msg: "{{ result.stdout }}"

```


License
-------

MIT

Author Information
------------------

- [@syndr](https://github.com/syndr/)

