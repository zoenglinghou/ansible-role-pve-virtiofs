pve_virtiofs
=========

Setup `virtiofs` shares between Proxmox PVE host and guest VM.

⚠️ Note: Due to this [limitation](https://github.com/ansible-collections/community.general/issues/1641), `args` for the VM are not set using `community.general.proxmox_kvm`, but with `ansible.builtin.lineinfile`. This could erase your previous `args`. **Proceed at your own discretion**.

Inspired by [yaro014 on Proxmox Forum](https://forum.proxmox.com/threads/virtiofs-support.77889/) and [Arch Wiki's `QEMU` Entry](https://wiki.archlinux.org/title/QEMU#Host_file_sharing_with_virtiofsd).

Requirements
------------

- `community.general.proxmox_kvm` collection
- `ansible.posix` collection
- `proxmoxer` Python module on PVE host

Role Variables
--------------

⚠️ Note: A lot of defaults **NEED** to be changed.

- `share_name`: Name of the `virtiofs` share. Default: `virtiofs`
- `socket`: `vhost-user UNIX domain socket `virtiofs` listens on. Default: `/var/run/vm101-vhost-fs.sock`
- `node`
  - `host`: Ansible host corresponding to the Proxmox host machine. Default: `proxmox`
  - `hostname`: Hostname of the Proxmox system. Default: `proxmox`
  - `share_dir`: Directory to share with the guest VM on the host machine. Default: `/mnt/data`
- `guest`
  - `host`: Ansible host corresponding to the guest VM. Default: `guest`
  - `hostname`: Hostname of the VM in Proxmox. Default: `guest`
  - `id`: VM ID. Default: `101`
  - `mount_point`: Mount point of the file share inside the VM. Default: `/mnt/share`
  - `mem_size`: Memory allotted to the VM. Default: `2G`
- `api_user`: Username to login Proxmox. Default: `root`
- `api_password`: Password for the corresponding Proxmox user. Default: `api_password`
- `api_token_id`: Token ID to log into Proxmox. Default: `api_token_id`
- `api_token_secret`: Token Secret for the corresponding Token. Default: `api_token_secret`

Example Playbook
----------------

```yaml
- hosts: pve_node, pve_guest
  roles:
  - name: zoenglinghou.pve_virtiofs
    vars:
      share_name: pve_share
      socket: /var/run/vm101-vhost-fs.sock
      node:
        host: pve_node
        hostname: pve_node
        share_dir: /mnt/data/pve_guest
      guest:
        host: pve_guest
        hostname: pve_guest
        id: 101
        mount_point: /mnt/share
        mem_size: 2G
      api_user: root@pam
      api_password: password
      api_token_id: ansible_0
      api_token_secret: xxx-xxx-xxx
```

License
-------

GPL-3.0-only
