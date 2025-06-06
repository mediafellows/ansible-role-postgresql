[![Ansible-Role Tests](https://github.com/mediafellows/ansible-role-postgresql/actions/workflows/ansible_test.yml/badge.svg)](https://github.com/mediafellows/ansible-role-postgresql/actions/workflows/ansible_test.yml)

## Ansible Role - PostgreSQL

Ansible role which installs and configures PostgreSQL, extensions, databases and users.

**This is just a fork** of https://github.com/ANXS/postgresql

#### Installation

This has been tested on Ansible 2.4.0 and higher.

To install:

```
ansible-galaxy install mediafellows.postgresql
```

#### Example Playbook

Including an example of how to use your role:

    - hosts: postgresql-server
      become: yes
      roles:
         - { role: mediafellows.postgresql }

#### Dependencies

None

#### Variables

```yaml
# Basic settings
postgresql_version: 12
postgresql_encoding: "UTF-8"
postgresql_locale: "en_US.UTF-8"
postgresql_ctype: "en_US.UTF-8"

postgresql_admin_user: "postgres"
postgresql_default_auth_method: "peer"

postgresql_service_enabled: false # should the service be enabled, default is true

postgresql_cluster_name: "main"
postgresql_cluster_reset: false

# List of databases to be created (optional)
# Note: for more flexibility with extensions use the postgresql_database_extensions setting.
postgresql_databases:
  - name: foobar
    owner: baz          # optional; specify the owner of the database
    hstore: yes         # flag to install the hstore extension on this database (yes/no)
    uuid_ossp: yes      # flag to install the uuid-ossp extension on this database (yes/no)
    citext: yes         # flag to install the citext extension on this database (yes/no)
    encoding: "UTF-8"   # override global {{ postgresql_encoding }} variable per database
    lc_collate: "en_GB.UTF-8"   # override global {{ postgresql_locale }} variable per database
    lc_ctype: "en_GB.UTF-8"     # override global {{ postgresql_ctype }} variable per database

# List of database extensions to be created (optional)
postgresql_database_extensions:
  - db: foobar
    extensions:
      - hstore
      - citext

# List of users to be created (optional)
postgresql_users:
  - name: baz
    pass: pass
    role_attr_flags: 'SUPERUSER,CREATEDB' # more options see https://docs.ansible.com/ansible/latest/modules/postgresql_user_module.html#parameter-role_attr_flags
    encrypted: yes  # if password should be encrypted, postgresql >= 10 does only accepts encrypted passwords

# List of schemas to be created (optional)
postgresql_database_schemas:
  - database: foobar           # database name
    schema: acme               # schema name
    state: present

  - database: foobar           # database name
    schema: acme_baz           # schema name
    owner: baz                 # owner name
    state: present
```

There's a lot more knobs and bolts to set, which you can find in the [defaults/main.yml](./defaults/main.yml)


#### License

Licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
