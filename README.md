# Ansible Role Redmine

## Prerequisites

* SGBD: Mysql
* apache2

## Usage example

    $> cat requirements.yml
    ---
    - src: https://github.com/geerlingguy/ansible-role-apache
    - src: https://github.com/goloxy/ansible-role-mysql
      version: origin/deveryware

    $> cat playbook.yml
    ---
    - name: Redmine servers - Installation
      hosts: sandbox-redmine*
      roles:
      - role: ansible-role-mysql
      - role: ansible-role-apache
        apache_packages:
        - apache2
        - apache2-utils
        - libapache2-mod-passenger
        apache_remove_default_vhost: True
        apache_create_vhosts: False
      - role: ansible-role-redmine
        redmine_sgbd: 'mysql'
        redmine_install: 'git'
        redmine_http_service: 'apache2-passenger'


## License

MIT

## Author Information

This role was created in 2016 by Olivier Locard on the behalf of Deveryware.

