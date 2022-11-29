Role Name
=========

This role is used to install NixOS by SSH using the ISO image.

Requirements
------------

* A running NixOS live system with ssh enabled
* Your SSH keys installed on the remote host

Role Variables
--------------

```yaml
# Installation
install_device: /dev/sda # The device the use to install
boot_on_zfs: true        # For instance, only ZFS is supported

# Hardening
hardened_kernel: true    # Do we rebuild the package to include SELinux?
hardened_fs: true        # Do we set noexec attributes on partitions?
hardened_auditing: true  # Do we install auditd?

# Usage
zsh_default_shell: true  # Use zsh instead of bash
ssh_enabled: true        # Start openssh on startup
ssh_password_auth: true  # Allow passwords

# Packages
system_packages: []      # Packages installed systemwise
users_packages:          # Packages reachable by the users
- nload
- vim
- tmux
- tcpdump
```

Dependencies
------------

N.A.

Example Playbook
----------------

This example generates `configuration.nix` using the given parameters:
```yaml
- name: Install NixOS
  hosts: all
  user: nixos
  become: true
  become_method: sudo
  gather_facts: false
  roles:
  - role: ansible-nixos-installer
  vars:
    hardened_kernel: false
    accounts:
    - username: mathieu
      extra_groups:
      - "wheel"
      ssh_keys:
      - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIkybLMMI3B5nNpr7lOBgxbcz06uQNe69d1elYnWQyPx mathieu"
```

This example generates imports the given `configuration.nix`:
```yaml
- name: Install NixOS
  hosts: all
  user: nixos
  become: true
  become_method: sudo
  gather_facts: false
  roles:
  - role: ansible-nixos-installer
  vars:
    configuration_nix: /my/path/configuration.nix
```

This example patches a running system with a new provided configuration:
```yaml
- name: Patch a live NixOS
  hosts: all
  user: my-admin-user
  become: true
  become_method: sudo
  gather_facts: false
  roles:
  - role: ansible-nixos-installer
  vars:
    patch_live_system: true
    configuration_nix: /my/path/configuration.nix
```

License
-------

GPL v3+

Author Information
------------------

Mathieu GRZYBEK
