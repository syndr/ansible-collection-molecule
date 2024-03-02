# Ansible Collection - syndr.molecule

This collection is intended to facilitate the creation and use of test environments using the Ansible [Molecule project](https://ansible.readthedocs.io/projects/molecule/).

It provides tooling to create Molecule testing scenarios via the `syndr.molecule.init` role, and test platforms via the `syndr.molecule.docker_platform` role.

When utilizing an image with systemd support (systemd packages are installed, etc.), the `docker_platform` role supports the creation of Docker containers with a functional Systemd implementation, which can be used to test Ansible code that makes use of Systemd services or related functionality.

# Using this collection

The host from which this collection is run (workstation, CI instance, etc.) must meet the following requirements:

* Molecule [is installed](https://ansible.readthedocs.io/projects/molecule/installation/) and is executable by the user.
* Ansible is installed, and `ansible-playbook` is executable by the user.
  * Ansible can be installed via `pip`, or using many system package managers.
* Docker should be installed
* The user running Molecule should be a member of the `docker` group (able to run `docker` commands without using `sudo`)

In general, Molecule can be installed with the following:

```bash
pip install molecule ansible-lint
```

Ansible can also be installed via `pip` if desired:
```bash
pip install ansible-core
```

Docker CE can be installed by following the appropriate [installation instructions](https://docs.docker.com/engine/install/) for your OS.

