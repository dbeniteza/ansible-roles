oracledb_exporter
=========

Description
------------

The role `oracledb_exporter` creates a container with Prometheus exporter image for Oracle database.
After adding this container as a target to Prometheus configuration (please check `prometheus_configuration` role), you can visualize collected metrics with specific Grafana dashboards available [here]( https://grafana.com/grafana/dashboards/3333-oracledb/) for Oracle database.

The container image is based on [iamseth/oracledb_exporter](https://github.com/iamseth/oracledb_exporter).
The container is named `<database>_oracledb_exporter`, being `<database>` the database instance name, and it listens on host port 9161 by default. Please check defaults (oracledb_exporter/defaults/main.yml) for further details.

Requirements
------------

The role requires a specific volume for the container where to store the custom metrics file (oracledb_exporter/files/custom-metrics.toml).
The aim to have this configuration is to have a reusable volume for all your Oracle databases where the exporter will collect the same metrics defined on the custom metrics file.

Role Variables
--------------

- **process**: Process to execute ['create','list_containers']. You can choose between create an oracledb_exporter container or list the existing oracledb_exporter containers. Default: "create"
- **database**: Database instance name to gather metrics with the oracledb_exporter. Default: "mydbInstance"
- **db_user**: Database username to collect metrics. Used for container environment variable DATA_SOURCE_NAME.  Default: "sys"
- **db_password**: Database user password. Used for container environment variable DATA_SOURCE_NAME.
- **db_role**: Database user role. Used for container environment variable DATA_SOURCE_NAME. Default: "sysdba"
- **db_host**: Database host. Used for container environment variable DATA_SOURCE_NAME.
- **db_port**: Database port. Used for container environment variable DATA_SOURCE_NAME. Default: "2008"
- **DATA_SOURCE_NAME**: Environment variable for the container with database connection details following Easy Connect naming method. Default: "{{ db_user }}/{{ db_password }}@//{{ db_host }}:{{ db_port }}/{{ database }}?as={{ db_role }}"
- **metrics_volume**: Volume to store Prometheus metrics. Used to create oracledb_exporter container. Default: "metrics_prometheus"
- **custom_metrics_filename**: Custom metrics filename.
- **container_name**: Container name. Used to create oracledb_exporter container. Default: "{{ database }}_oracledb_exporter"
- **container_port**: Container port. Used to create oracledb_exporter container. Default: "9161"
- **container_image**: oracledb_exporter container image. Default: "iamseth/oracledb_exporter"

Dependencies
------------

N/A

Example Playbook
----------------

```yaml
- name: Create oracledb_exporter container
  hosts: myContainersHost
  roles:
    - role: oracledb_exporter
      vars:
        ansible_python_interpreter: auto
        ansible_interpreter_python_fallback: ['/usr/bin/python3','/usr/bin/python2','/usr/bin/python']
        process: "create"
        database: "mydbInstance" # MODIFY this value to set database instance name
        db_user: "sys"
        db_password: "mydbPassword" # MODIFY this value to set database user password
        db_role: "sysdba"
        db_host: "mydbHost"
        db_port: "2008"
        metrics_volume: "metrics_prometheus"
        custom_metrics_filename: "custom-metrics.toml"
        container_port: "9161"
        container_image: "iamseth/oracledb_exporter"
        DATA_SOURCE_NAME: "{{ db_user }}/{{ db_password }}@//{{ db_host }}:{{ db_port }}/{{ database }}?as={{ db_role }}"
      tags: oracledb_exporter
```

Example output
----------------

```
PLAY [all] *********************************************************************

TASK [Gathering Facts ] ********************************************************
ok: [myContainersHost]

TASK [oracledb_exporter : Validating arguments against arg spec 'main' - This role creates a oracledb_exporter container argument_spec={'process': {'type': 'str', 'required': False, 'description': "Process to execute ['create','list_containers']. You can choose between create an oracledb_exporter container or list the existing oracledb_exporter containers", 'choices': ['create', 'list_containers'], 'default': ['create']}, 'database': {'type': 'str', 'required': True, 'description': 'Database instance name to gather metrics with the oracledb_exporter.'}, 'db_user': {'type': 'str', 'required': False, 'description': 'Database username. Used for container environment variable DATA_SOURCE_NAME.', 'default': 'sys'}, 'db_password': {'type': 'str', 'required': False, 'description': 'Database user password. Used for container environment variable DATA_SOURCE_NAME.', 'default': 'mydbPassword'}, 'db_role': {'type': 'str', 'required': False, 'description': 'Database user role. Used for container environment variable DATA_SOURCE_NAME.', 'default': 'sysdba'}, 'db_host': {'type': 'str', 'required': False, 'description': 'Database host. Used for container environment variable DATA_SOURCE_NAME.', 'default': 'mydbHost'}, 'db_port': {'type': 'str', 'required': False, 'description': 'Database port. Used for container environment variable DATA_SOURCE_NAME.', 'default': '2008'}, 'DATA_SOURCE_NAME': {'type': 'str', 'required': False, 'description': 'Environment variable for the container with database connection details following Easy Connect naming method.', 'default': 'user/password@//myhost:2008/?as=sysdba'}, 'metrics_volume': {'type': 'str', 'required': False, 'description': 'Volume to store Prometheus metrics. Used to create oracledb_exporter container.', 'default': 'metrics_prometheus'}, 'custom_metrics_filename': {'type': 'str', 'required': False, 'description': 'Custom metrics filename.', 'default': 'custom-metrics.toml'}, 'container_name': {'type': 'str', 'required': False, 'description': 'Container name. Used to create oracledb_exporter container.', 'default': 'mydbInstance_oracledb_exporter'}, 'container_port': {'type': 'str', 'required': False, 'description': 'Container port. Used to create oracledb_exporter container.', 'default': '9161'}, 'container_image': {'type': 'str', 'required': False, 'description': 'oracledb_exporter container image', 'default': 'iamseth/oracledb_exporter'}}, provided_arguments={}, validate_args_context={'type': 'role', 'name': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/oracledb_exporter', 'argument_spec_name': 'main', 'path': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/oracledb_exporter'}] ***
ok: [myContainersHost]

TASK [oracledb_exporter : Check if container exists for mydbInstance instance] *****
ok: [myContainersHost]

TASK [oracledb_exporter : ansible.builtin.debug msg=Container not found for mydbInstance instance. Creating a new one] ***
ok: [myContainersHost] => {
    "msg": "Container not found for mydbInstance instance. Creating a new one"
}

TASK [oracledb_exporter : Gather info about specific volume for metrics] *******
ok: [myContainersHost]

TASK [oracledb_exporter : Copy custom metrics file custom-metrics.toml to remote machine src=/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/oracledb_exporter/files/custom-metrics.toml, dest=/var/lib/containers/storage/volumes/metrics_prometheus/_data/custom-metrics.toml] ***
ok: [myContainersHost]

TASK [oracledb_exporter : Create oracledb_exporter container] ******************
changed: [myContainersHost]

TASK [oracledb_exporter : ansible.builtin.debug msg=started mydbInstance_oracledb_exporter with ID fa8247fab05745e8ad4aa7f6ff7b51393da5564c692641a5f0f85a22a7ba73e1
] ***
ok: [myContainersHost] => {
    "msg": "started mydbInstance_oracledb_exporter with ID fa8247fab05745e8ad4aa7f6ff7b51393da5564c692641a5f0f85a22a7ba73e1\n"
}

PLAY RECAP *********************************************************************
myContainersHost : ok=8    changed=1    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   
```

License
-------

BSD

Author Information
------------------

Daniel Benítez Águila
