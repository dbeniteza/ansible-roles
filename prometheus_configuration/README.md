prometheus_configuration
=========

The role `prometheus_configuration` configures Prometheus to add a container target that serves as exporter.
First, it checks whether a container exists with the specified name, then creates a block template in memory with the details of the target. After that, it updates Prometheus configuration file (prometheus.yaml) to include the block with the new target and finally reloads Prometheus configuration to visualize the target on the console.

Requirements
------------

The role requires a existing container with a configured exporter for Prometheus.
It works also with a server or virtual machine because it is only required to specify the name and port where the exporter listens in `containers_server` and `container_port` variables.

Role Variables
--------------

- **exporter_type**: Prometheus exporter type. You can set any exporter type you want because the template for the exporter (`roles/prometheus_configuration/templates/exporter.j2`) is reusable and this variable is only used to build the `container_name` variable (which value you can modify as well) and to set a tag in the exporter template.
Please bear in mind that if you aim to use another kind of exporter, you will need to add the new exporter type to the arguments validation file (`roles/prometheus_configuration/meta/argument_specs.yml`). Default: "oracledb_exporter"
- **element**: Configuration element. It must be a database instance name for exporter_type='oracledb_exporter'. Default: "mydbInstance"
- **containers_server**: Containers server. Used to create exporter template.
- **container_name**: Container name. Used to create exporter template.  Default: "{{ element }}_{{ exporter_type }}"
- **container_port**: Container port. Used to create exporter template.  Default: "9161"
- **prometheus_server**: Prometheus server. It will store the exporter templates.
- **prometheus_server_url**: Prometheus server URL. Used to reload configuration after adding the exporters.  Default: "http://{{prometheus_server}}:9090/-/reload"
- **prometheus_config_dir**: Prometheus server configuration directory. Used to modify Prometheus configuration file. By default, this config directory can be found at `/etc/prometheus`. If you are using a Prometheus container, you need to specify here the disk mounted on that route (/etc/prometheus).  Default: "/prometheus-docker"
- **prometheus_config_file**: Prometheus server configuration file.  Default: "prometheus.yml"

Dependencies
------------

N/A

Example Playbook
----------------

```yaml
- name: Configures a exporter in a Prometheus server from an existing container.
  hosts: myContainersHost
  roles:
    - role: prometheus_configuration
      vars:
        ansible_python_interpreter: auto
        ansible_interpreter_python_fallback: ['/usr/bin/python3','/usr/bin/python2','/usr/bin/python']
        exporter_type: "oracledb_exporter"
        element: "mydbInstance" # MODIFY this value to set database instance name
        containers_server: "myContainersHost" # MODIFY this value to set the appropiate containers server
        container_name: "{{ element }}_{{ exporter_type }}" # MODIFY this value to set the appropiate container name
        container_port: "9161"
        prometheus_server: "myPrometheusServer" # MODIFY this value to set the appropiate prometheus server
        prometheus_server_url: "http://{{prometheus_server}}:9090/-/reload"
        prometheus_config_dir: "/prometheus-docker" # MODIFY this value to set the appropiate prometheus configuration directory
        prometheus_config_file: "prometheus.yml"
      tags: prometheus_configuration
```

Example output
----------------

```
PLAY [all] *********************************************************************

TASK [Gathering Facts ] ********************************************************
ok: [myContainersHost]

TASK [prometheus_configuration : Validating arguments against arg spec 'main' - This role configures a exporter from an existing container in a Prometheus server. argument_spec={'exporter_type': {'type': 'str', 'required': False, 'description': 'Prometheus exporter type.', 'choices': ['oracledb_exporter'], 'default': ['oracledb_exporter']}, 'element': {'type': 'str', 'required': True, 'description': "Configuration element. It must be a database instance name for exporter_type='oracledb_exporter'."}, 'containers_server': {'type': 'str', 'required': False, 'description': 'Containers server. Used to create exporter template.'}, 'container_name': {'type': 'str', 'required': False, 'description': 'Container name. Used to create exporter template.', 'default': '9161'}, 'container_port': {'type': 'str', 'required': False, 'description': 'Container port. Used to create exporter template.', 'default': '9161'}, 'prometheus_server': {'type': 'str', 'required': False, 'description': 'Prometheus server. It will store the exporter templates.'}, 'prometheus_server_url': {'type': 'str', 'required': False, 'description': 'Prometheus server URL. Used to reload configuration after adding the exporters.'}, 'prometheus_config_dir': {'type': 'str', 'required': False, 'description': 'Prometheus server configuration directory. Used to modify configuration.'}, 'prometheus_config_file': {'type': 'str', 'required': False, 'description': 'Prometheus server configuration file.', 'default': 'prometheus.yml'}}, provided_arguments={}, validate_args_context={'type': 'role', 'name': 'prometheus_configuration', 'argument_spec_name': 'main', 'path': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/prometheus_configuration'}] ***
ok: [myContainersHost]

TASK [prometheus_configuration : Check if container mydbInstance_oracledb_exporter exists] ***
ok: [myContainersHost]

TASK [prometheus_configuration : ansible.builtin.debug msg=Container mydbInstance_oracledb_exporter already exists] ***
ok: [myContainersHost] => {
    "msg": "Container mydbInstance_oracledb_exporter already exists"
}

TASK [prometheus_configuration : Setting fact exporter_block (template in memory) exporter_block=  - job_name: 'mydbInstance_oracledb_exporter'
    # metrics_path defaults to '/metrics'
    static_configs:
      - targets: ['myContainersHost:9161'] 
        labels:
          name: oracledb_exporter
          instance: mydbInstance
          stage: des] ***
ok: [myContainersHost -> myPrometheusServer]

TASK [prometheus_configuration : Rebuild prometheus.yaml base file from exporters] ***
changed: [myContainersHost -> myPrometheusServer]

TASK [prometheus_configuration : Reload prometheus configuration (only if config file has been modified) url=http://myPrometheusServer:9090/-/reload, method=POST, status_code=200] ***
ok: [myContainersHost -> myPrometheusServer]

PLAY RECAP *********************************************************************
myContainersHost : ok=7    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0  
```

License
-------

BSD

Author Information
------------------

Daniel Benítez Águila
