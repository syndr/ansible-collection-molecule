syndr.molecule.init
=========

Initialize the Molecule testing framework for a project.

TODO: Add example howtouse info

Requirements
------------

1. Molecule should be installed and executable from a location in the users PATH
1. Docker should be installed
1. The current user should be a member of the `docker` group

Role Variables
--------------

```yaml
# The type of project that this Molecule configuration will be integrated into
init_project_type: role

# Filesystem location of the molecule scenario being initialized
init_scenario_dir: "{{ molecule_scenario_directory }}"

# The filesystem location of the project being tested by this Molecule configuration
#  - default value assumes that your Molecule project is located at <project dir>/extensions/molecule/<scenario>
init_project_dir: "{{ molecule_scenario_directory.split('/')[:-3] | join('/') }}"

# The container image that should be used for this platform
#  - note that variable substitution can be used as described here: https://ansible.readthedocs.io/projects/molecule/configuration/#variable-substitution
init_platform_image: "geerlingguy/docker-${MOLECULE_DISTRO:-rockylinux9}-ansible:latest"

# Does the specified image include SystemD support?
init_platform_systemd: true
```

Dependencies
------------

**Collections**  
- community.docker
- syndr.molecule

Example Playbook
----------------

TBA

License
-------

MIT

Author Information
------------------

- Vincent Oltion ([@syndr](https://github.com/syndr/))

