Role Name
=========

Role to build docker-compose files from many services.

Requirements
------------

`docker-compose` is required to be installed before run the role. If you want only to generate compose files, it could be optional.

Role Variables
--------------

`docker_services: []` the services to mount docker-compose. It could be read from a var file and include dynamically.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - name: Build Docker Compose for Apps Group
      hosts: localhost
      become: no

      vars:
        docker_compose_version: '3.4'
        services_enabled:
          - rundeck
        services_group: apps
        services_vars_path: "{{ playbook_dir }}/vars/docker-services/{{ services_group }}/"

      roles:
        - mtulio.docker-services

License
-------

BSD

Author Information
------------------

Marco Tulio R Braga.
