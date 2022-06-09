ansible-k8s-cloudsql-standalone
===============================

Este rol permite el despliegue de ['CloudSQL Proxy'](https://cloud.google.com/sql/docs/postgres/sql-proxy?hl=es) en un **clúster de Kubernetes** en modo Standalone que puede ser utilizado por los pods o servicios implementados dentro del clúster.

Requirements
------------

- GCloud SDK >= 375.0.0
- kubectl
- Ansible >= 2.9
- Python 3
- Unzip
- Ansible Collections:
  - name: cloud.common
  - name: community.docker
  - name: community.general
  - name: community.kubernetes
  - name: kubernetes.core

Role Variables
--------------

Este rol reaquiere la definición de las variables de conexión hacia la instancia Google CloudSQL.

```yml
cloud_sql_name: "foo"
cloud_sql_instance: "foo-instance"
```

Se requiere definir que imagen de CloudSQL Proxy y el namespace de Kubernetes donde se desplegará (si no existe se creará).

```yml
cloud_sql_image: "gcr.io/cloudsql-docker/gce-proxy:1.14"
namespace: "default"
```

Se reaueire definir el puerto de origen y destino del servicio.

```yml
src_port: 5432
target_port: 5432
```

Se requiere referenciar las credenciales (cuenta de servicio) con permisos para acceder a la instancia CloudSQL (que puede estar incluso en otro proyecto de GCP).

```yml
cloud_sql_credentials: ""
```

Dependencies
------------

Clúster de Kubernetes implementado.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

- *Ejemplo-1. Implementación estándar y unitaria*

```yml
- hosts: localhost
  remote_user: root
  roles:
    - ansible-k8s-cloudsql-standalone
  vars:
    cloud_sql_credentials: "ewqeweewrwerqwerewrwe..."
    cloud_sql_name: "cloudsql-lipiapp-dev"
    cloud_sql_instance: "lipigas-eb269:us-east1:db-lipiapp-5c23ef73=tcp:0.0.0.0:5432"
    cloud_sql_image: "gcr.io/cloudsql-docker/gce-proxy:1.14"
    namespace: "cloudsql-proxy"
```

- *Ejemplo-2. Incluyendo el rol dentro del task (esto permite invocar el rol múltiple veces y con varibales diferenciadas)*

```yml
- hosts: localhost
  remote_user: root
  vars:
    cloud_sql_credentials: "{{ lookup('env', 'EMPLIPIGAS_DATALAKE_DATA_MANAGER_ACCOUNT_KEY') }}"
    cloud_sql_image: "gcr.io/cloudsql-docker/gce-proxy:1.14"
    namespace: "cloudsql-proxy"

  tasks:
    - name: Deploy CloudSQL Dev
      include_role:
        name: ansible-k8s-cloudsql-standalone
      vars:
        cloud_sql_name: "cloudsql-lipiapp-dev"
        cloud_sql_instance: "lipigas-eb269:us-east1:db-lipiapp-5c23ef73=tcp:0.0.0.0:5432"
      tags: lipiapp-cloudsql-dev

    - name: Deploy CloudSQL otro ambiente o proyecto
      include_role:
        name: ansible-k8s-cloudsql-standalone
      vars:
        cloud_sql_name: "cloudsql-lipi-otro"
        cloud_sql_instance: "lipigas-proyecto:us-west1:db-lipi-otro-asdasd3=tcp:0.0.0.0:5432"
      tags: lipiapp-cloudsql-otro
```

Test
----

```ansible-playbook tests/test.yml -i tests/inventory  --syntax-check```

License
-------

BSD

Author Information
------------------

Role creado para Lipigas por Marcelo Cárdenas (hackwish).
