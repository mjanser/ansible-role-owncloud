# Ansible Role: owncloud

An Ansible role that installs owncloud on Fedora using nginx.

## Requirements

For this role `nginx` and `php` have to be installed beforehand. To achieve this the following roles can be used:
- mjanser.nginx
- mjanser.php

## Role Variables

Available variables are listed below, along with default values:

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
    #  - enabled: true

    owncloud_version: 9.0.0beta2
    owncloud_release_channel: testing

    owncloud_data_directory: /var/lib/owncloud/data

    #owncloud_ssl_certificate: "/etc/pki/tls/certs/{{ owncloud_server_name }}.crt"
    #owncloud_ssl_certificate_key: "/etc/pki/tls/private/{{ owncloud_server_name }}.key"

    owncloud_database_server: mysql # mysql, sqlite, manual
    owncloud_database_name: owncloud
    owncloud_database_username: owncloud
    owncloud_database_password: secret

    owncloud_restore_database: ~
    owncloud_restore_config: ~

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

## License

MIT

## TDOD
- test mysql import
