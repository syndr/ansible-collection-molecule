syndr.molecule.prepare_controller
=========

Prepare a Molecule controller to run tests using local project repositories.

This can be used on a workstation for development purposes, or in the context of a CI host for testing.

Requirements
------------

Project formats currently supported by this role are:  
* `role`

Role Variables
--------------

```yaml
# The filesystem location of the project being tested by this Molecule configuration
#  - default value assumes that your Molecule project is located at <project dir>/molecule/<scenario>
prepare_controller_project_dir: "{{ molecule_scenario_directory.split('/')[:-2] | join('/') }}"

# The working directory to which the current project should be linked
#  - used for running tests against roles that are not located in the default system locations
#    IE: within a git project directory or monolith
prepare_controller_role_dir: "{{ molecule_ephemeral_directory }}/roles"

# The working directory to which the current project should be linked
#  - used for running tests against projects that are not located in the default system locations
#    IE: within a git project directory or monolith
prepare_controller_collection_dir: "{{ molecule_ephemeral_directory }}/collections"

# Type of Ansible project which will be tested by this Molecule deployment
prepare_controller_project_type: auto
```

Dependencies
------------

None

Example Playbook
----------------

```yaml
- name: Prepare controller for execution
  hosts: localhost
  tasks:
    - name: Configure for standalone role testing
      ansible.builtin.include_role:
        name: syndr.molecule.prepare_controller
      vars:
        prepare_controller_project_type: role
```

License
-------

MIT

Author Information
------------------

- [@syndr](https://github.com/syndr/)

