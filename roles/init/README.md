syndr.molecule.init
=========

Initialize the Molecule testing framework for a project.

# Usage

To use this role, do the following:

## Create the molecule scenario directory

```bash
mkdir -p molecule/default
```

## Create the init playbook

Within the new scenario directory (default), create a file `init.yml` containing the example playbook from this README.

Configuration variables for the `init` role can be customized as desired.

You should now have a directory structure similar to the following:
```
ansible-role-users
├── defaults
├── molecule
│   └── default
│       └── init.yml
├── handlers
├── LICENSE
├── meta
├── README.md
├── tasks
├── templates
└── vars
```

## Run the init playbook

```bash
ansible-playbook molecule/default/init.yml
```

You should now see that additional configuration has been added to the `default` scenario directory:  
```
ansible-role-users
├── defaults
├── molecule
│   └── default
│       ├── collections.yml
│       ├── converge.yml
│       ├── create.yml
│       ├── destroy.yml
│       ├── init.yml
│       ├── molecule.yml
│       ├── prepare.yml
│       ├── requirements.yml
│       └── verify.yml
├── handlers
├── LICENSE
[...]
```

## Run Molecule to verify initial setup

Your base molecule scenario should now be deployed and ready to use. To verify this, you can run molecule against the new scenario:

```bash
molecule test
```

Note: The `molecule` command must be run from within the extensions directory. Furthermore, if you are creating additional scenarios with names other `default`, you will need to include the scenario name when running Molecule. This can be done as:  
```bash
molecule test -s my_other_scenario
```

Molecule should run through its test process and exit successfully. Success should be apparent from the lack of red text in the output, but can also be verified from the shell by checking the exit code of the `molecule` command:

```bash
❯ echo $?                                         
0
```

Requirements
------------

1. Molecule should be installed and executable from a location in the users PATH
1. Ansible should be installed, with `ansible-playbook` executable via the users PATH
1. Docker should be installed
1. The current user should be a member of the `docker` group

Role Variables
--------------

```yaml
# The type of project that this Molecule configuration will be integrated into
init_project_type: role

# Filesystem location of the molecule scenario being initialized
init_scenario_dir: "{{ molecule_scenario_directory | default(playbook_dir) }}"

# The filesystem location of the project being tested by this Molecule configuration
#  - default value assumes that your Molecule project is located at <project dir>/molecule/<scenario>
init_project_dir: "{{ init_scenario_dir.split('/')[:-2] | join('/') }}"

# The container image that should be used for this platform
#  - Variable substitution can be used as described here: https://ansible.readthedocs.io/projects/molecule/configuration/#variable-substitution
init_platform_image: "geerlingguy/docker-${MOLECULE_DISTRO:-rockylinux9}-ansible:latest"

# Does the specified image include SystemD support?
init_platform_systemd: true

# Create backups of any files that would be clobbered by running this role
init_file_backup: true

# Path to the ansible secret file that should be used by the Molecule test
#  - Variable substitution can be used as described here: https://ansible.readthedocs.io/projects/molecule/configuration/#variable-substitution
#  - Set to "" to disable
init_ansible_secret_path: ""
```

Dependencies
------------

**Collections**  
- community.docker
- syndr.molecule

Example Playbook
----------------

```yaml
---
# Initialize a Molecule scenario for use within a role

- name: Provision file structure
  hosts: localhost
  tasks:
    - name: Launch provisioner
      ansible.builtin.include_role:
        name: syndr.molecule.init
      vars:
        init_project_type: role
        init_platform_image: "geerlingguy/docker-${MOLECULE_DISTRO:-rockylinux9}-ansible:latest"
        init_platform_systemd: true
```

License
-------

MIT

Author Information
------------------

- [@syndr](https://github.com/syndr/)

