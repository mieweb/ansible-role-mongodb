# Ansible Role: MongoDB Replicaset (Debian)

This role installs and configures a MongoDB replica set on Debian-based systems, supporting dynamic grouping for multiple replica sets and secure authentication. Designed for use in LXC containers and other environments.

## Role Variables

The following variables can/should be customized by an administrator:

| Variable                    | Default            | Description                                                      |
|-----------------------------|--------------------|------------------------------------------------------------------|
| `mongodb_version`           | `8.0`              | MongoDB version to install                                       |
| `mongodb_replicaset_name`   | `rs0`              | Name of the replica set for this host                            |
| `mongodb_port`              | `27017`            | MongoDB port                                                     |
| `mongodb_bind_ip`           | `0.0.0.0`          | Bind IP address for mongod                                       |
| `mongodb_admin_user`        | `admin`            | Initial admin username (for authentication)                      |
| `mongodb_admin_password`    | `change_me`      | Initial admin password (for authentication)                      |

### Advanced/Environment Variables

| Variable                    | Default            | Description                                                      |
|-----------------------------|--------------------|------------------------------------------------------------------|
| `mongodb_conf_file`         | `/etc/mongod.conf` | Path to mongod configuration file                                |
| `mongodb_data_dir`          | `/var/lib/mongodb` | Data directory for MongoDB                                       |
| `mongodb_log_dir`           | `/var/log/mongodb` | Log directory for MongoDB                                        |

## Dynamic Replica Set Grouping

Hosts are grouped dynamically by their `mongodb_replicaset_name` variable. All hosts with the same value will form a replica set together.

## WiredTiger Cache Sizing

The role sets `storage.wiredTiger.engineConfig.cacheSizeGB` to 50% of the container's total memory, using the Ansible fact `ansible_memtotal_mb`.

## Authentication

Authentication is enabled by default. The admin user is created after replica set initiation. Change the default password before deploying to production.

## Example Playbook

```yaml
- hosts: all
  become: yes
  vars:
    mongodb_replicaset_name: "rs0"
    mongodb_admin_password: "yourStrongPassword"
  roles:
    - role: ansible-role-mongodb
```

## License
MIT

## Author
Robert Gingras <rgingras@mieweb.com>
