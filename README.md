# Ansible Role: MongoDB Enterprise Advanced 4.x STIG Hardening

This repository contains an Ansible role for hardening MongoDB based on [DISA's](https://public.cyber.mil/stigs/downloads/) MongoDB Enterprise Advanced 4.x Security Technical
Implementation Guide (STIG) Version 1, Release 2. (Applies to database versions 4, 5, 6, & 7)

## Requirements

- Ansible
- Python 3.9+
- MongoDB Enterprise 4.x, 5.x, 6.x, 7.x

## Install

You can install the `mitre.mongo-stig` role via Ansible Galaxy:

```
ansible-galaxy install mitre.mongo-stig
```

Alternatively, you can link directly to the GitHub repository in your `requirements.yml`:

```yaml
# requirements.yml
roles:
  - name: mitre.mongo-stig
    src: https://github.com/mitre/ansible-mongodb-enterprise-advanced-4-stig-hardening
    version: main
```

## Role Variables

These variables offer additional configuration options during the installation of the role, which are located in `defaults/main.yml`. Any changes to these variables should be specified in the `vars` section of your playbook.

### CONFIGURATION FLAGS

| Variable             | Description                                                                                              |
| -------------------- | -------------------------------------------------------------------------------------------------------- |
| `prep_conf`          | Enable preparation of configuration files. Defaults to `true`.                                           |
| `enterprise_edition` | Enable Enterprise Edition features. Defaults to `true`.                                                  |
| `fips_mode`          | Enable FIPS mode. Defaults to `true`.                                                                    |
| `mongostig_cat1`     | Enable MongoDB STIG Category 1 settings. Defaults to `true`.                                             |
| `mongostig_cat2`     | Enable MongoDB STIG Category 2 settings. Defaults to `true`.                                             |
| `encryption_at_rest` | Enable encryption at rest. Set to `true` if any data is PII, classified, or deemed necessary to encrypt. |
| `kmip_enabled`       | Enable KMIP for encryption at rest. Defaults to `false`.                                                 |

### CONNECTION VARIABLES

| Variable                   | Description                                                                                                                                                |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `mongo_owner`              | Owner of MongoDB files. Defaults to `root`.                                                                                                                |
| `mongo_group`              | Group of MongoDB files. Defaults to `root`.                                                                                                                |
| `mongo_dba`                | MongoDB DBA user. Defaults to `root`.                                                                                                                      |
| `mongo_dba_password`       | Password for MongoDB DBA user. Defaults to `root`.                                                                                                         |
| `mongo_host`               | Hostname for MongoDB. Defaults to `localhost`.                                                                                                             |
| `mongo_port`               | Port for MongoDB. Defaults to `27017`.                                                                                                                     |
| `mongo_auth_source`        | Authentication database for MongoDB. Defaults to `admin`.                                                                                                  |
| `max_incoming_connections` | Maximum number of incoming connections. Defaults to `800000`.                                                                                              |
| `authentication_mechanism` | Authentication mechanisms for MongoDB. Defaults to`SCRAM-SHA-256`. Other possible values [here](https://www.mongodb.com/docs/manual/core/authentication/). |

### ROLES AND USERS

| Variable                         | Description                                                                        |
| -------------------------------- | ---------------------------------------------------------------------------------- |
| `mongo_admin_roles`              | Roles for MongoDB admin. Example: `["root"]`.                                      |
| `mongo_super_users`              | Super users for MongoDB. Example: `["admin.root"]`.                                |
| `mongo_users`                    | Users for MongoDB. Example: `["test.myTester", "products.myRoleTestUser"]`.        |
| `inappropriate_mongo_privileges` | List of inappropriate privileges. Example: `["changeStream", "createCollection"]`. |

### FILE PATHS

| Variable                     | Description                                                                                 |
| ---------------------------- | ------------------------------------------------------------------------------------------- |
| `mongo_permissions`          | Permissions for MongoDB files. Defaults to `0600`.                                          |
| `mongod_config_path`         | Path to MongoDB configuration file. Defaults to `/etc/mongod.conf`.                         |
| `audit_log_destination`      | Destination for audit logs. Set to either `file` or `syslog`. Defaults to `file`.           |
| `mongo_audit_directory_path` | Directory path for MongoDB audit logs. Defaults to `/var/log/mongodb/audit/`.               |
| `mongo_audit_file_path`      | File path for MongoDB audit logs. Defaults to `/var/log/mongodb/audit/auditLog.bson`.       |
| `certificate_key_file_dest`  | Destination path for MongoDB certificate key file. Defaults to `/etc/ssl/mongodb.pem`.      |
| `certificate_key_file_src`   | Source path for MongoDB certificate key file. Defaults to `../../certificates/mongodb.pem`. |
| `ca_file_dest`               | Destination path for CA bundle file. Defaults to `/etc/ssl/CA_bundle.pem`.                  |
| `ca_file_src`                | Source path for CA bundle file. Defaults to `../../certificates/dod_CAs.pem`.               |
| `data_file_directory_path`   | Directory path for MongoDB data files. Defaults to `/data/db/`.                             |

### ENCRYPTION SETTINGS

| Variable                       | Description                                           |
| ------------------------------ | ----------------------------------------------------- |
| `encryption_cipher_mode`       | Cipher mode for encryption. Defaults to `AES256-GCM`. |
| `KMIP_server_host_name`        | KMIP server hostname.                                 |
| `KMIP_server_port`             | KMIP server port.                                     |
| `KMIP_server_ca_file`          | CA file for KMIP server.                              |
| `KMIP_client_certificate_file` | Client certificate file for KMIP.                     |
| `security_encryption_key_file` | Path to encryption key file.                          |

### AUDIT SETTINGS

| Variable       | Description                                                                                                                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `mongo_filter` | Filter for MongoDB auditing. Example: `'{ atype: { $in: [ "createCollection", "dropCollection" ] } }'`. More information [here](https://www.mongodb.com/docs/manual/tutorial/configure-audit-filters/). |

## Dependencies

- `mitre.yedit` role
- `community.mongodb` collection

## Example Playbook

```yaml
# playbook.yml
- hosts: localhost
  roles:
    - role: mitre.mongo-stig
      vars:
        fips_mode: true
        enterprise_edition: true
```

## License

- See the [License](/LICENSE.md)

## Notice

- See the [Notice](/NOTICE.md)

## Authors

- Sean Chacon Cai - [seanlongcc](https://github.com/seanlongcc)

## Special Thanks

- Will Dower - [wdower](https://github.com/wdower)
