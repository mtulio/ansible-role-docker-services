docker-services
===============

Role to build docker-compose files from many services.

Requirements
------------

`docker-compose` is required to be installed before run the role. If you want only to generate compose files, it could be optional.

Role Variables
--------------

```yaml
docker_services_workdir: /tmp
docker_services_group: default
docker_services_enabled: []      # the services to mount docker-compose file. It could be read from a var file and include dynamically.
docker_services_vars_path: ''

docker_compose_version: 3.4
docker_compose_file: "{{ docker_services_workdir }}/docker-compose-{{ services_group }}.yml"

```

Example Playbook
----------------

### Sample 01

Create a docker-compose file with Databases with these steps:
1. create the service definition on path (as example bellow) `{{ playbook_dir }}/vars/docker-services/arangodb.yml`
2. create any file dependencies (directories and files that should exists in the destination) - any of those the `files` option will resolve the dependencies
3. Create the playbook as bellow:

```yaml
- name: Build Docker Compose for DBs Group
  hosts: localhost
  become: no

  vars:
    docker_services_workdir: /cloud
    docker_compose_version: '3.4'
    docker_services_group: dbs
    docker_services_vars_path: "{{ playbook_dir }}/vars/docker-services/"
    docker_compose_file: "{{ docker_services_workdir }}/docker-compose-{{ docker_services_group }}.yml"
    docker_services_enabled:
      - name: arangodb
        files:
          - src: "{{ playbook_dir }}/files/docker-services/arangodb/conf"
            dest: "{{ docker_services_workdir }}/arangodb/conf"
            type: directory
          - dest: "{{ docker_services_workdir }}/dbs/arangodb/apps"
            type: directory-present
          - dest: "{{ docker_services_workdir }}/dbs/arangodb/db"
            type: directory-present
      - name: prometheus
        files:
          - src: "{{ playbook_dir }}/files/docker-services/prometheus/"
            dest: "{{ docker_services_workdir }}/prometheus"
            type: directory
          - dest: "{{ docker_services_workdir }}/dbs/arangodb"
            type: directory-present
      - name: rdmysql
        files:
          - dest: "{{ docker_services_workdir }}/dbs/mysql-rundeck"
            type: directory-present

  roles:
    - mtulio.docker-services
```

The result will be the docker-compose file `/cloud/docker-compose-dbs.yml`

License
-------

BSD

Author Information
------------------

Marco Tulio R Braga.
