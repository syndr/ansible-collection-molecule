syndr.molecule.prepare_controller
=========

Prepare a Molecule controller to run tests using local project repositories.

This can be used on a workstation for development purposes, or in the context of a CI host for testing.

Requirements
------------

This role attempts to discover what type of project it is being utilized in, and applies specific configuration as needed. However, the `prepare_controller_project_type` variable can also be set if an explicit project type configuration is desired.

Project formats currently supported by this role are:  
* `role`
* `collection`
* `monolith`

## Galaxy meta configuration

When used with a role or collection, the Galaxy meta information for the role must be configured!


### Standalone roles

Location: `{{ role_dir }}/meta/main.yml`

For standalone roles that are not part of a collection, the minimum required information is:
```yaml
galaxy_info:
  author: you
  role_name: cool_stuff
```

It is however strongly recommended that the entire file be updated with the correct information for your role!

### Collections

Location: `{{ collection_dir }}/galaxy.yml`

The following minimum values should be defined:  
* `namespace`
* `name`
* `version`

It is however also strongly recommended that the entire file be updated with the correct information for your collection! 

### Monoliths

A monolith is a project that contains both roles and playbooks in the same directory structure.

This collection supports the following file structure:  
```
.
├── ansible.cfg
├── collections
│   ├── ansible_collections
│   └── requirements.yml
├── inventory
├── molecule
│   └── default
│       ├── [...]
│       ├── collections.yml
│       ├── converge.yml
│       ├── molecule.yml
│       └── verify.yml
├── playbooks
│   ├── configure_toaster
│   │   ├── main.yml -> rick.yml
│   │   ├── README.md
│   │   ├── rick.yml
│   │   ├── tasks
│   │   └── vars
│   ├── configure_server
│   │   ├── main.yml
│   │   ├── README.md
│   │   ├── tasks
│   │   └── templates
│   ├── [...]
│   └── install_things
│       ├── install-cheese.yml
│       ├── tasks
│       └── vars
├── README.md
├── roles
│   ├── powerlevel_9000
│   │   ├── defaults
│   │   ├── meta
│   │   ├── README.md
│   │   ├── tasks
│   │   ├── tests
│   │   └── vars
│   ├── disks
│   │   ├── defaults
│   │   ├── handlers
│   │   ├── meta
│   │   ├── README.md
│   │   ├── tasks
│   │   ├── templates
│   │   ├── tests
│   │   └── vars
│   ├── [...]
│   └── something_else
└── scripts
```

When using this collection within a monolithic repository, make sure that any `ansible.cfg` configuration includes the default ansible locations, or that this collection is installed within the repository.

For example, `ansible.cfg` should contain something like:  
```yaml
[defaults]
roles_path = ./roles:/usr/share/ansible/roles:/etc/ansible/roles:~/.ansible/roles
collections_path = ./collections:/usr/share/ansible/collections:~/.ansible/collections
```

or this collection should be installed locally with:  
```bash
ansible-galaxy collection install -p ./collections syndr.molecule
```

or if your `collections/requirements.yml` includes this collection:
```bash
ansible-galaxy collection install -p ./collections -r ./collections/requirements.yml
```

#### Testing roles within a monolithic project

When configuring molecule testing for individual roles within a monolithic project (creating a `roles/<role_name>/molecule` directory), take care _not_ to name the scenario "default", as there is already a "default" scenario for the monolithic project itself if you have created `molecule/default` as described above! Instead, name your role scenario with a unique name.

```bash
ROLE_NAME=your_role
mkdir -p molecule/role-$ROLE_NAME
wget -P molecule/role-$ROLE_NAME https://raw.githubusercontent.com/syndr/ansible-collection-molecule/main/roles/init/files/init.yml
ansible-playbook molecule/role-$ROLE_NAME/init.yml
```

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

