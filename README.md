Artifactory Role
================

Install Artifactory Pro / Oss on Ubutnu / Centos via rpm/apt
Centos + Pro fully tested ATM + Mariadb [ Client requiest ;)]

Requirements
------------
mariadb - you can use with [Ansible-role-mariadb](https://github.com/shelleg/ansible-role-mariadb)
mysql - you can use with `geerlingguy.mysql`


Role Variables
--------------

Artifactory basics:

* artifactory_flavor: `pro`
* artifactory_version: `5.2.1`
* artifactory_user: `artifactory`
* artifactory_group: `artifactory`

Artifactory directories:

* artifactory_runtime_dir: /opt/jfrog/artifactory/
... see more in defaults/main.yml

Database / Storage engine:
* artifactory_database_engine: `mariadb`
* artifactory_create_db: `true` change to false if managed outside Ansible ...

Allow a more generic way of managing database with db_ prefixed variables like so:
* artifactory_db_root_user: `"{{ db_root_user | d('root') }}"`
* artifactory_db_root_pass: `"{{ db_root_pass | d('Changem3') }}"`
* artifactory_db_name:      `"{{ db_name | d('arti') }}"`
* artifactory_db_user:      `"{{ db_user | d('arti') }}"`
* artifactory_db_pass:     `"{{ db_pass | d('arti') }}"`
* artifactory_db_host:      `"{{ db_host | d('127.0.0.1') }}"`
* artifactory_db_port:      `"{{ db_port | d('3306') }}"`
* artifactory_db_encoding:  `"{{ db_encoding | d('utf8') }}"`

Configure nginx reverse proxy:
* artifactory_reverse_proxy: `true`
* artifactory_https_port: `443`
* artifactory_proxy_port: `80`
* artifactory_host_name: `"{{ inventory_hostname }}"`
* artifactory_max_artifact_size: `'5120M'`

Configure Tomcat http and ajp ports in server.xml 
* artifactory_http_port: `8081`
* artifactory_ajp_port: `8019` 

Dependencies
------------

* java role - sued ansiblebit.oracle-java in [Ansible-playbook-artifactory](https://github.com/shelleg/ansible-playbook-artifactory) 


Example Playbook
----------------

Single host with mariadb + artifactory + nging http reverse proxy: 

    - hosts: artifactory
      become: yes
      # vars: see -> ./group_vars/all/00_general.yml
      roles:
        - role: ansible-role-set-resolver
        - role: shelleg.epel
        - role: mariadb
        - role: oracle-java
          oracle_java_set_as_default: yes
        - role: artifactory

Group vars:
    
    mariadb_artifactory_optimzed: true
    artifactory_database_engine: mariadb
    artifactory_db_name: artifactory
    artifactory_db_user: artifactory
    artifactory_db_pass: root
    artifactory_db_host: localhost
    artifactory_db_port: 3306
    
    db_root_user: root
    db_root_pass: myCustomPass
    
    mariadb_db_properties:
      - db_name: "{{ artifactory_db_name }}"
        db_user: "{{ artifactory_db_user }}"
        db_pass: "{{ artifactory_db_pass }}"
        db_priv: '{{ artifactory_db_name }}.*:ALL'
        db_host: "{{ artifactory_db_host }}"
        db_state: present
    
    # this is a sample not a rela lisence ;)
    artifactory_lisence: "jb12tcBozB4dBgY0LDP0IWUrKFAkV5ZeJFbZfKFUJO1FnqEjrLud+fBTik5XJ/9hGW
    8zuII0g6Y44qTI0ZOgCwPFIA6Mkv2G0xX3IvzfklWd7BEt5c8AH3xANvu2EVL7YHocBTE8Kizf9P
    QF07lQrL1mhr/B+h7i1ACEkjjyr7Z5XfaQsHJ7dX78SnZce9vKwbDFVDVTxy4DkzkT3visN
    x1AtsSjrlqy5/ZKmuHRVaJevJ7itgsdVsRW7hTBD/fsdOwxjcD1N1+eaen
    U1p3kwSs87w2py7cLBF6Te/MLSelC5HcVxxgweQK8WGBDmvrpe210Wp7aDkdT3lnZqNZ3bAVKJau
    lJwuIg3rU7AoidC9caULsXi6F81OShWeSm07Iqms6Isdfseq+VFVn6mDtbQ72MD4M97FYi3LSWDi
    H+NiZrntBNJb9nfsKcKatRVL+K0Z7ZnPiqSbLCg5xVI4nyZAQlQ="


Changelog:
----------

* initial release - initial release support ubutnu 14/16.04 && centos 6/7

License
-------

[Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0)

Author Information
------------------

Haggai Philip Zagury <hagzag@tikalk.com> part of
[Shellg](https://github.com/shelleg/shelleg) project.
see also [Shellg Docs](http://shelleg.github.io/shellegDoc/)
