fmw_infra
=========

The role `fmw_infra` installs Oracle Fusion Middleware Infrastructure in a remote machine with a JDK installation.

Requirements
------------

The role requires a existing Java JDK installation in the remote machine. It also requires a existing HTTP repository server to download the binaries from. Set server details in `repository_xxx` variables (check fmw_infra/defaults/main.yml).

Role Variables
--------------

- **process**: Process to execute ['install','deinstall']. You can choose install Oracle FMW Infrastructure or deinstall it. Default: "install"
- **fmw_version**: Oracle FMW Infrastructure version to install. Default: "12.2.1.4.0"
- **oracle_home**: ORACLE_HOME directory. Default: "/var/tmp/oracle_home"
- **java_home**: JAVA_HOME directory. Default: "/tmp/jdk"
- **oraInventory**: oraInventory directory. Default: "/var/tmp/oraInventory"
- **create_oraInventory**: Create oraInventory file (oracle software inventory installed). Default: "true"
- **user**: Installation owner username. It must exists on the remote machine. Default: "oracle"
- **group**: Installation owner group name. It must exists on the remote machine. Default: "oracle"
- **repository_server**: HTTP repository server that hosts binaries.
- **repository_server_port**: HTTP repository server port. Default: "8080"
- **download_url**: URL used to download Oracle FMW Infra binaries. Default: "{{repository_server_url}}/oracle/fmw/binaries/{{fmw_version}}/fmw_{{fmw_version}}_infrastructure.jar"

Dependencies
------------

N/A

Example Playbook
----------------

```yaml
- name: Install FMW Infrastructure
  hosts: myRemoteHost
  roles:
    - role: fmw_infra
      vars:
        ansible_python_interpreter: auto
        ansible_interpreter_python_fallback: ['/usr/bin/python3','/usr/bin/python2','/usr/bin/python']
        process: install
        fmw_version: "12.2.1.4.0"
        oracle_home: "/var/tmp/oracle_home"
        java_home: /admin/jdk/jdk8
        oraInventory: /var/tmp/oraInventory
        create_oraInventory: true
        user: oracle
        group: oracle
        repository_server: "myHTTPRepoServer"
      tags: fmw_infra
```


Example output
----------------

```
PLAY [Install FMW Infrastructure] ***********************************************************************************************************************************************************************************************************

TASK [Install FMW Infrastructure name={{ lookup('env', 'NODECONF_HOME')}}/roles/fmw_infra] **************************************************************************************************************************************************

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Validating arguments against arg spec 'main' - This role installs Oracle Fusion Middleware Infrastructure in a remote machine with a JDK installation. argument_spec={'process': {'type': 'str', 'required': False, 'description': "Process to execute ['install','deinstall']. You can choose install Oracle FMW Infrastructure or deinstall it.", 'choices': ['install', 'deinstall'], 'default': ['install']}, 'fmw_version': {'type': 'str', 'required': False, 'description': 'Oracle FMW Infrastructure version to install.', 'default': '12.2.1.4.0'}, 'oracle_home': {'type': 'str', 'required': False, 'description': 'ORACLE_HOME directory.', 'default': '/var/tmp/oracle_home'}, 'java_home': {'type': 'str', 'required': False, 'description': 'JAVA_HOME directory.', 'default': '/tmp/jdk'}, 'oraInventory': {'type': 'str', 'required': False, 'description': 'oraInventory directory.', 'default': '/var/tmp/oraInventory'}, 'create_oraInventory': {'type': 'bool', 'required': False, 'description': 'Create oraInventory file (oracle software inventory installed).', 'default': True}, 'user': {'type': 'str', 'required': False, 'description': 'Installation owner username. It must exists on the remote machine.', 'default': 'oracle'}, 'group': {'type': 'str', 'required': False, 'description': 'Installation owner group name. It must exists on the remote machine.', 'default': 'oracle'}, 'repository_server': {'type': 'str', 'required': False, 'description': 'HTTP repository server that hosts binaries.'}, 'repository_server_port': {'type': 'str', 'required': False, 'description': 'HTTP repository server port.', 'default': '8080'}, 'download_url': {'type': 'str', 'required': False, 'description': 'URL used to download Oracle FMW Infra binaries.', 'default': 'http://myHTTPRepoServer:8080/oracle/fmw/binaries/12.2.1.4.0/fmw_12.2.1.4.0_infrastructure.jar'}}, provided_arguments={}, validate_args_context={'type': 'role', 'name': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra', 'argument_spec_name': 'main', 'path': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra'}] ***
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Check oracle_home /var/tmp/oracle_home] ********************************************************************************************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Create oraInventory dir /var/tmp/oraInventory] *************************************************************************************************************
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Create oraInst.loc] ****************************************************************************************************************************************
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Create tmp directory] **************************************************************************************************************************************
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Check if binary is present in repository] ******************************************************************************************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Download from HTTP server] *********************************************************************************************************************************
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Create response file /tmp/fmw_infra_install-HNbLJm/fmw_infr.rsp dest=/tmp/fmw_infra_install-HNbLJm/fmw_infr.rsp, src=fmw_infr.rsp.j2] **********************
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Install software _raw_params=/admin/jdk/jdk8/bin/java  -jar /tmp/fmw_infra_install-HNbLJm/fmw_12.2.1.4.0_infrastructure.jar -responseFile /tmp/fmw_infra_install-HNbLJm/fmw_infr.rsp  -silent -invPtrLoc /var/tmp/oraInventory/oraInst.loc -logLevel Info, _uses_shell=True, _ansible_check_mode=False, _ansible_no_log=False, _ansible_debug=False, _ansible_diff=False, _ansible_verbosity=0, _ansible_version=2.13.6, _ansible_module_name=ansible.legacy.command, _ansible_syslog_facility=LOG_USER, _ansible_selinux_special_fs=['fuse', 'nfs', 'vboxsf', 'ramfs', '9p', 'vfat'], _ansible_string_conversion_action=warn, _ansible_socket=None, _ansible_shell_executable=/bin/sh, _ansible_keep_remote_files=False, _ansible_tmpdir=None, _ansible_remote_tmp=$HOME/.ansible/tmp] ***
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/fmw_infra : Remove tmp dir state=absent, dest=/tmp/fmw_infra_install-HNbLJm, _ansible_check_mode=False, _ansible_no_log=False, _ansible_debug=False, _ansible_diff=False, _ansible_verbosity=0, _ansible_version=2.13.6, _ansible_module_name=ansible.builtin.file, _ansible_syslog_facility=LOG_USER, _ansible_selinux_special_fs=['fuse', 'nfs', 'vboxsf', 'ramfs', '9p', 'vfat'], _ansible_string_conversion_action=warn, _ansible_socket=None, _ansible_shell_executable=/bin/sh, _ansible_keep_remote_files=False, _ansible_tmpdir=None, _ansible_remote_tmp=$HOME/.ansible/tmp] ***
changed: [myRemoteHost]

PLAY RECAP **********************************************************************************************************************************************************************************************************************************
myRemoteHost              : ok=10   changed=7    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0
```

License
-------

BSD

Author Information
------------------

Daniel Benítez Águila
