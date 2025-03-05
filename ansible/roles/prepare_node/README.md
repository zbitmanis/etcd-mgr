# Ansible Role: prepare_node

## Description
The `prepare_node` Ansible role is designed to prepare a basic node for further feature installation. It includes tasks for installing necessary packages, configuring time synchronization, and setting up users, groups, and directories.

## Author
- **Author:** Andris Zbitkovskis
- **License:** MIT

## Requirements
- **Ansible version:** 2.9 or higher
- **Supported OS:** Debian, Ubuntu

## Role Variables

The role provides the following variables for customization, defined in `defaults/main.yaml`:

### Packages
```yaml
prepare_node_debian_packages:
  - name: 'bash-completion'
  - name: 'ca-certificates'
  - name: 'curl'
  - name: 'chrony'
  - name: 'nvme-cli'
  - name: 'tmux'
  - name: 'vim'
  - name: 'podman'
```
List of Debian packages to be installed.

### Time Synchronization
```yaml
prepare_node_ntp_server: 'ch.pool.ntp.org'
prepare_node_config_chrony: true
```
- `prepare_node_ntp_server`: NTP server for time synchronization.
- `prepare_node_config_chrony`: Whether to configure Chrony.

### System Groups
```yaml
prepare_node_system_groups:
  - name: etcd
    gid: 1001
```
System groups to be created.

### System Users
```yaml
prepare_node_system_users:
  - name: etcd
    group: etcd
    uid: 1001
    shell: /bin/nologin
    home: /var/etcd
    create_home: false
```
System users to be created.

### System Directories
```yaml
prepare_node_system_folders:
  - path: /var/etcd
    mode: '0750'
    user: etcd
    group: etcd
  - path: /etc/etcd/tls
    mode: '0750'
    user: etcd
    group: etcd
  - path: /var/lib/etcd
    mode: '0700'
    user: etcd
    group: etcd
  - path: /var/log/etcd
    mode: '0750'
    user: etcd
    group: etcd
```
System directories to be created with their permissions.

### Host Entries
```yaml
prepare_node_hosts: {}
```
Entries for the `/etc/hosts` file.

## Tasks

The main tasks are defined in `tasks/main.yaml` and include:

1. **Debian/Ubuntu specific tasks:**
   - Installing repository management tools.
   - Installing useful packages.
2. **Time synchronization:**
   - Configuring Chrony.
   - Enabling the Chrony service.
3. **System setup:**
   - Creating system groups.
   - Creating system users.
   - Creating system directories.
4. **Host entries:**
   - Adding host entries to `/etc/hosts`.

## Dependencies
There are no dependencies for this role.

## Galaxy Info
```yaml
role_name: prepare_node
galaxy_name: zbitmanis
galaxy_tags: [node]
```

## Usage

Include this role in your playbook:

```yaml
- hosts: nodes
  roles:
    - role: prepare_node
```

You can override default variables in your playbook or inventory file.

## License
This project is licensed under the MIT License.


