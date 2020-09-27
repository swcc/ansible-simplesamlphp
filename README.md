SimpleSAMLphp Ansible role
=========

[![Build Status](https://travis-ci.org/swcc/ansible-simplesamlphp.svg?branch=master)](https://travis-ci.org/swcc/ansible-simplesamlphp) [![Ansible Galaxy](https://img.shields.io/badge/role-swcc.ansible__simplesamlphp-blue.svg)](https://galaxy.ansible.com/swcc/ansible-simplesamlphp)

Installs simplesamlphp from [simplesamlphp.org servers](https://simplesamlphp.org/download) sources. This role assumes you will run simplesamlphp with `PHP-FPM` and thus installs it for you as an ansible role dependency (with the [`NBZ4live.php-fpm`](https://github.com/NBZ4live/ansible-php-fpm) role.

Example Playbook
----------------

Basic example playbook:

```yaml
- hosts: webservers
  roles:
    - role: swcc.simplesamlphp
      simplesamlphp_destination: /home/simplesamlphp
      simplesamlphp_version: 1.19.0
```

A more complete example playbook installing a identity provider together with a service provider configured is visible in the test file `tests/test.yml`.

Role parameters
----------------

| Variable                                            | Default    | Type            | Description                                                                                                                        |
| -----------------------                             | :------:   | :-------------: | ------------                                                                                                                       |
| `simplesamlphp_version`                             | `latest`   | `string`        | Which simplesamlphp version to install                                                                                             |
| `simplesamlphp_destination`                         | `/var/www` | `string`        | Where to install SimpleSAMLphp (will be installed in "{{ simplesamlphp_destination}}/simplesamlphp/" directory on your filesystem) |
| `simplesamlphp_dir_user`                            | `www-data` | `string`        | Which unix user should own the installed directory                                                                                 |
| `simplesamlphp_dir_group`                           | `www-data` | `string`        | Which unix group should own the installed directory                                                                                |
|-----------------------------------------------------|------------|-----------------|------------------------------------------------------------------------------------------------------------------------------------|
| `simplesamlphp_identity_provider`                   |            | `object`        | Configuration of the IdP                                                                                                           |
|-----------------------------------------------------|------------|-----------------|------------------------------------------------------------------------------------------------------------------------------------|
| `simplesamlphp_identity_provider.cert`              |            | `object`        | Configuration of the IdP certificate                                                                                               |
| `simplesamlphp_identity_provider.cert.name`         |            | `string`        | certificate name                                                                                                                   |
| `simplesamlphp_identity_provider.cert.country_code` |            | `string`        | certificate country code                                                                                                           |
| `simplesamlphp_identity_provider.cert.state`        |            | `string`        | certificate state                                                                                                                  |
| `simplesamlphp_identity_provider.cert.city`         |            | `string`        | certificate city                                                                                                                   |
| `simplesamlphp_identity_provider.cert.org_name`     |            | `string`        | certificate organisation name                                                                                                      |
| `simplesamlphp_identity_provider.cert.common_name`  |            | `string`        | certificate common name (domain name)                                                                                              |
|-----------------------------------------------------|------------|-----------------|------------------------------------------------------------------------------------------------------------------------------------|
| `simplesamlphp_identity_provider.auth.name`         |            | `string`        | IdP authentication name                                                                                                            |
| `simplesamlphp_identity_provider.auth.type`         |            | `string`        | IdP authentication module                                                                                                          |
| `simplesamlphp_identity_provider.auth.config`       |            | `object`        | IdP authentication configuration                                                                                                   |
|-----------------------------------------------------|------------|-----------------|------------------------------------------------------------------------------------------------------------------------------------|
| `simplesamlphp_service_providers`                   |            | `list`          | Service Providers                                                                                                                  |
|-----------------------------------------------------|------------|-----------------|------------------------------------------------------------------------------------------------------------------------------------|

_⚠️ Please also check the php-fpm variables of the dependent [php-fpm ansible role](https://github.com/NBZ4live/ansible-php-fpm#role-variables) before running this current role. ⚠️_

Most importantly check the php version you want to run by setting the `php_fpm_version` variable. Here is an example configuration of the `php-fpm` dependent role which should suit most needs:

```yaml
php_fpm_version: 7.4

php_fpm_pool_defaults:
  pm: dynamic
  pm.max_children: 10
  pm.start_servers: 2
  pm.min_spare_servers: 1
  pm.max_spare_servers: 4
php_fpm_pools:
  - name: www
    user: www-data
    group: www-data
    listen: "/run/php/php{{ php_fpm_version }}-fpm.sock"
    listen.owner: www-data
    listen.group: www-data
    chdir: /var/www
    env:
      PATH: "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin"
      TMPDIR: "/tmp"
      TMP: "/tmp"
      HOSTNAME: "$HOSTNAME"
```

Makefile for easier Ansible usage
------------------

I have written a small Makefile to make your future ansible runs easier. Don't hesitate to [check it out](https://github.com/paulRbr/ansible-makefile/).

Download the `*.deb` package from the github releases, install it and start using it with `ansible-make help`.

License
-------

GPLv3
