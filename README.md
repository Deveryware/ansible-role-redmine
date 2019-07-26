# Ansible Role Redmine

## Prerequisites

* SGBD: Mysql
* apache2

## Usage example

    $> cat requirements.yml
    ---
    - src: https://github.com/geerlingguy/ansible-role-apache
    - src: https://github.com/zzet/ansible-rbenv-role.git
    - src: https://github.com/goloxy/ansible-role-mysql
      version: origin/deveryware

    $> cat playbook.yml
    ---
    - name: Redmine servers - Installation
      hosts: sandbox-redmine*
      roles:
      vars:
        rbenv:
          env: user
          version: v1.1.2
          default_ruby: 2.6.3
          rubies:
          - version: 2.6.3
      roles:
      - role: ansible-rbenv-role
        rbenv_users:
        - redmine
      - role: ansible-role-mysql
      - role: ansible-role-apache
        apache_packages:
        - apache2
        - apache2-dev
        - apache2-utils
        - libapache2-mod-passenger
        - libapache2-mpm-itk
        - passenger
        - passenger-dev
        - w3m
        apache_remove_default_vhost: True
        apache_create_vhosts: False
        apache_mods_enabled:
        - passenger.conf
        - passenger.load
      - role: ansible-role-redmine
        redmine_sgbd: 'mysql'
        redmine_install: 'git'
        redmine_http_service: 'apache2-passenger'
        redmine_sub_uri: '/redmine'
        redmine_ruby_env_init: '/bin/bash /etc/profile.d/rbenv.sh'
        redmine_ruby_path: '"/usr/share/redmine/.rbenv/shims/ruby'

### email_delivery

```
    redmine_email_delivery_method: :smtp
    redmine_email_smtp_settings:
      - address: smtp.company.eu
      - domain: company.eu
      - port: 25
```

## License

MIT

## Author Information

This role was created in 2016 by Olivier Locard on the behalf of Deveryware.

