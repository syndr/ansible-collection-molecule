molecule.linode_platform
=========

Create an Akamai Linode platform for Molecule.

This role is intended to be used via the `molecule.platform` role that is included with this collection, and should not be referenced directly in a playbook.

Configuration is done via the `platforms` section of the `molecule.yml` file in your Molecule scenario directory.

Required configuration options are:

- `name`: The name of the platform (string)
- `type`: `linode`
- `image`: The Linode image ID to use for the instance (string, e.g., "linode/ubuntu22.04")
- `region`: The Linode region to deploy the instance in (string, e.g., "us-east")
- `type_spec`: The Linode instance type specification (string, e.g., "g6-nanode-1")

Optional configuration options are:

- `boot_wait_seconds`: The number of seconds to wait for the instance to boot (integer, default: 10)
- `instance_creation_timeout`: The timeout in seconds for instance creation (integer, default: 300)
- `private_key_path`: The path to the private key file for SSH (string, default: `<molecule_ephemeral_directory>/id_rsa`)
- `public_key_path`: The path to the public key file for SSH (string, default: `<molecule_ephemeral_directory>/id_rsa.pub`)
- `ssh_user`: The SSH user to use for connecting to the instance (string, default: "root")
- `ssh_port`: The SSH port to use for connecting to the instance (integer, default: 22)
- `stackscript_id`: The ID of a StackScript to run on instance creation (string, default: "")
- `stackscript_data`: Data to pass to the StackScript (dictionary, default: {})
- `tags`: A dictionary of tags to apply to the instance (dictionary, default: {})

Requirements
------------

**Python Modules**
- `linode_api4` (>= 5.29.0) - Official Python library for Linode API v4
- `polling` - Required for asynchronous operations
- `ansible-specdoc` (>= 0.0.19) - Required by linode.cloud modules

## Linode API Configuration

These tests require a Linode API token with the following permissions:

- **Linodes**: Read/Write access
- **SSH Keys**: Read/Write access

To create an API token:
1. Log in to the [Linode Cloud Manager](https://cloud.linode.com/)
2. Navigate to your profile → API Tokens
3. Click "Create a Personal Access Token"
4. Set the required permissions and expiry
5. Save the token securely - it will only be shown once

Role Variables
--------------

In order to connect to Linode, you will need the following environment variable to be set:

```bash
export LINODE_API_TOKEN="your-linode-api-token-here"
```

This token must have appropriate permissions to create, list, and destroy Linode instances and SSH keys.

Full role configuration options are available in the [defaults/main.yml](defaults/main.yml) file.

Dependencies
------------

**Collections**
- `linode.cloud`

Example Playbook
----------------

This role is intended to be used via the `molecule.platform` role that is included with this collection, and should not be referenced directly in a playbook.

Configuration is done via the `platforms` section of the `molecule.yml` file in your Molecule scenario directory.

```yaml
platforms:
  - name: test-ubuntu22
    type: linode
    image: linode/ubuntu22.04
    region: us-east
    type_spec: g6-nanode-1
    ssh_user: root
  - name: test-rocky9
    type: linode
    image: linode/rocky9
    region: us-east
    type_spec: g6-nanode-1
    ssh_user: root
```

To utilize this role, use the `platform` role that is included with this collection in your `create.yml` playbook!

```yaml
- name: Create
  hosts: localhost
  gather_facts: false
  tasks:
    - name: Create platform(s)
      ansible.builtin.include_role:
        name: syndr.molecule.platform

# We want to avoid errors like "Failed to create temporary directory"
- name: Validate that inventory was refreshed
  hosts: molecule
  gather_facts: false
  tasks:
    - name: Check uname
      ansible.builtin.raw: uname -a
      register: result
      changed_when: false

    - name: Display uname info
      ansible.builtin.debug:
        msg: "{{ result.stdout }}"

```


License
-------

MIT
