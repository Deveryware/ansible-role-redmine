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
      tasks:
      - copy:
          src: '/usr/share/doc/redmine/examples/apache2-passenger-host.conf'
          dest: '/etc/apache2/sites-available/redmine.conf'
          remote_src: True
        register: redmine_apache_sites_available
      - file:
          dest: '/etc/apache2/sites-enabled/redmine.conf'
          src: '/etc/apache2/sites-available/redmine.conf'
          state: link
      - name: Reload apache2 if needed.
        service: name=apache2 state=reloaded
        when: redmine_apache_sites_available.changed


## License

MIT

## Author Information

This role was created in 2016 by Olivier Locard on the behalf of Deveryware.

