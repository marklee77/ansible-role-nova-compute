nova controller ansible role
============================

The purpose of this role is to deploy nova-controller onto Ubuntu. 

Role Variables
--------------

- openstack_mysql_host: 127.0.0.1
- openstack_mysql_port: 3306
- openstack_rabbitmq_host: 127.0.0.1
- openstack_rabbitmq_port: 5672
- openstack_compute_package: nova-compute-kvm
- openstack_compute_virt_type: qemu
- openstack_log_verbose: true
- openstack_log_debug: false
- openstack_identity_region: RegionOne

Example Playbook
-------------------------

    - hosts: all
      sudo: True
      roles:
        - nova-controller

License
-------

GPLv2

Author Information
------------------

http://stillwell.me
