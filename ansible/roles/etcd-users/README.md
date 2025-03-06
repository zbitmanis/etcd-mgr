# Ansible Role: etcd-users

## Description
The `etcd-users` role is used to manage etcd users and roles, configuring user authentication and role-based access control in an etcd cluster. It automates the creation of users, roles, and the assignment of permissions, using Podman to interact with the etcd container.

## Requirements
- Ansible 2.9 or later
- etcd running in a Podman container
- TLS certificates for secure communication with etcd

## Role Variables

The following variables are defined in `defaults/main.yaml`:

| Variable                       | Description                                            | Default Value                          |
|---------------------------------|--------------------------------------------------------|----------------------------------------|
| `etcd_users_node_iface`         | Network interface for etcd node                        | `{{ etcd_node_iface }}`               |
| `etcd_users_container`          | Name of the etcd container                             | `etcd`                                |
| `etcd_users_cacert_file`        | Path to the etcd CA certificate                        | `/etc/etcd/tls/etcd-ca.pem`           |
| `etcd_user_root_password`       | Password for the root user, stored in Vault            | `{{ vault_etcd_user_root_password }}`  |
| `etcd_roles_list`               | List of roles to be created in etcd                    | See example below                     |
| `etcd_users_list`               | List of users with their roles and passwords           | See example below                     |

### Example: etcd roles and users
```yaml
etcd_roles_list:
  - role: atenant
    prefix: atenant
  - role: btenant
    prefix: btenant

etcd_users_list:
  - user: auser
    role: atenant
    password: '{{ vault_etcd_user_auser_password }}'
  - user: buser
    role: btenant
    password: '{{ vault_etcd_user_buser_password }}'


### Example Playbook

Below is an example of how to use the etcd-users role in your playbook:

```
- hosts: etcd_nodes
  roles:
    - role: etcd-users
      vars:
        etcd_users_node_iface: eth0
        etcd_user_root_password: '{{ vault_etcd_user_root_password }}'
        etcd_roles_list:
          - role: atenant
            prefix: atenant
        etcd_users_list:
          - user: auser
            role: atenant
            password: '{{ vault_etcd_user_auser_password }}'

```
