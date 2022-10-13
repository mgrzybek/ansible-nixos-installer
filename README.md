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

# Kernel and SELinux
hardened_kernel: true    # Do we rebuild the package to include SELinux?

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
      is_system_user: true
      ssh_keys:
      - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIkybLMMI3B5nNpr7lOBgxbcz06uQNe69d1elYnWQyPx mathieu"
```

License
-------

GPL v3+

Author Information
------------------

Mathieu GRZYBEK
