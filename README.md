ansible-k8s-cloudsql-standalone
=========

A brief description of the role goes here.

Requirements
------------

- GCloud SDK >= 375.0.0
- kubectl
- Ansible >= 2.9
- Python 3
- Unzip
- Ansible Collections:
  - name: ansible.posix
  - name: cloud.common
  - name: community.docker
  - name: community.general
  - name: community.kubernetes
  - name: kubernetes.core

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

Example Use
-----------

```
---
- hosts: localhost
  remote_user: root
  roles:
    - ansible-k8s-cloudsql-standalone
  vars:
    cloud_sql_credentials: "123asDDF"
    cloud_sql_name: "cloudsql-instance"
    cloud_sql_instance: "gcp-project:us-east1:db-instance-1234asdf=tcp:0.0.0.0:5432"
    cloud_sql_image: "gcr.io/cloudsql-docker/gce-proxy:1.14"
    namespace: "cloudsql-proxy"

```

Test
----

```ansible-playbook tests/test.yml -i tests/inventory  --syntax-check```
