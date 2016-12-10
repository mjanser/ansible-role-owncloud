# Ansible Role: owncloud

An Ansible role that installs nextCloud or ownCloud on Fedora using nginx.

## Requirements

For this role `nginx`, `mysql` (or `mariadb`) and `php` have to be installed beforehand. To achieve this the following roles can be used:
- mjanser.nginx
- mjanser.mysql
- mjanser.php

## Role Variables

Available variables are listed below, along with default values:

    owncloud_vendor: nextcloud

    owncloud_server_name: example.com

    owncloud_admin_username: admin
    owncloud_admin_password: secret

    owncloud_config_user_backend: ~ # imap, smb, ftp
    owncloud_config_user_backend_argument: ~

    owncloud_config_trusted_domains: ["localhost", "example.com"]
    owncloud_config_cli_url: "https://example.com"
    owncloud_config_mail_domain: "example.com"
    owncloud_config_defaultapp: ~
    owncloud_config_filesystem_check: 1
    owncloud_config_loglevel: 2

    owncloud_apps: []
    #  - name: news
    #    enabled: true

    owncloud_version: ~ # nextCloud: 10.0.2, ownCloud: 9.1.2

    owncloud_data_directory: /var/lib/owncloud/data

    #owncloud_ssl_certificate: "/etc/pki/tls/certs/{{ owncloud_server_name }}.crt"
    #owncloud_ssl_certificate_key: "/etc/pki/tls/private/{{ owncloud_server_name }}.key"

    owncloud_database_server: mysql # mysql, sqlite, manual
    owncloud_database_name: "{{ owncloud_vendor }}"
    owncloud_database_username: "{{ owncloud_vendor }}"
    owncloud_database_password: secret

    owncloud_restore_database: ~

## Dependencies

None

## Example Playbook

    - hosts: all
      roles:
        - { role: mjanser.owncloud }
      vars:
        owncloud_ssl_certificate_key: "/etc/pki/tls/certs/example.com.crt"
        owncloud_config_defaultapp: "gallery,files"
        owncloud_config_trusted_domains: ["localhost", "example.com"]
        owncloud_apps:
          - name: files_external

## Configuration

The configuration files are located in `/etc/nextcloud` or `/etc/owncloud`.
The default `config.php` is managed by nextCloud or ownCloud itself and can be changed manually or by using the cloud tools.
The file `custom.config.php` is managed by this ansible role and should not be changed manually.

You can also create your own configuration file in `/etc/nextcloud` or `/etc/owncloud` which overrides parameters from the other files.

## License

MIT
