# Ansible Role: owncloud

An Ansible role that installs nextCloud or ownCloud on Fedora using nginx.

## Requirements

For this role `nginx` and `php` have to be installed beforehand. To achieve this the following roles can be used:
- mjanser.nginx
- mjanser.php

If you want to MySQL or MariaDB as database, they also have to be installed first. You can use the role `mjanser.mysql` for that.

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

    owncloud_version: ~ # nextCloud: 11.0.0, ownCloud: 9.1.3

    owncloud_data_directory: /var/lib/owncloud/data

    #owncloud_ssl_certificate: "/etc/pki/tls/certs/{{ owncloud_server_name }}.crt"
    #owncloud_ssl_certificate_key: "/etc/pki/tls/private/{{ owncloud_server_name }}.key"

    owncloud_database_server: mysql # mysql, sqlite, manual
    owncloud_database_name: "{{ owncloud_vendor }}"
    owncloud_database_username: "{{ owncloud_vendor }}"
    owncloud_database_password: secret

    owncloud_restore_database: ~

### Vendor

With the variable `owncloud_vendor` you can define whether to install nextCloud or ownCloud.

### Server name

The variable `owncloud_server_name` defines the hostname which will be configured in the nginx configuration.
Make sure the server is accessable from the network with this name.

### Admin user

During the installation an admin user will be created. With the variables `owncloud_admin_username` and
`owncloud_admin_password` you can define the credentials for it. This user can be changed or even removed later.

### User backend

If you want to use a different user backend than the internal one, you can configure it with the variables
`owncloud_config_user_backend` and `owncloud_config_user_backend_argument`. Please look at the corresponding
documentation for more details.

### Configuration

The variables prefixed with `owncloud_config_` can be used to change some configuration parameters.
Please look at the corresponding documentation for more details.

### Apps

Apps can be enabled or disabled using the variable `owncloud_apps`. Be aware that those apps have to installed,
this ansible role doesn't install any apps.

### Version

With the variable `owncloud_version` you can define a specific version of nextCloud or ownCloud to install.
It defaults to the respective latest stable version.

### Data directory

The variable `owncloud_data_directory` defines the directory on the filesystem where the data will be stored.

### SSL

The nginx vhost is always configured to be encrypted. By default the certificate and key are taken from
`/etc/pki/tls/certs/{{ owncloud_server_name }}.crt` and `/etc/pki/tls/private/{{ owncloud_server_name }}.key`.
You can override those paths with the variables `owncloud_ssl_certificate` and `owncloud_ssl_certificate_key`.

### Database

The database connection can be configured with the variables prefixed by `owncloud_database_`.
The role currently supports `mysql` and `sqlite`.

If you specify a path to a SQL file in the variable `owncloud_restore_database`, it will be imported
when the database is created. This only works with MySQL/MariaDB.

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
