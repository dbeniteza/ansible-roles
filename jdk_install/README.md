jdk_install
=========

The role `jdk_install` installs Java JDK.
First, it checks whether a JDK installation exists on the specified JAVA_HOME, then downloads JDK binaries from HTTP repository server to the remote machine. After that, it installs JDK on the JAVA_HOME directory.

Requirements
------------

The role requires a existing HTTP repository server to download the binaries from. Set server details in `repository_xxx` variables (check jdk_install/defaults/main.yml).

Role Variables
--------------

- **process**: Process to execute ['install','deinstall']. You can choose install Java JDK or deintall it.Default: "install"
- **platform**: Destination machine OS platform. Used in conjunction with `jvm_type` and `version` variables to build the context and get binaries from HTTP repository server. Default: "solaris"
- **jvm_type**: JVM type. Example: 'hotspot', 'openjdk'. Default: "hotspot"
- **version**: JDK version. Default: "1.8.0_321"
- **install_dir**: Installation directory. JDK binaries are unarchived under this directory. Default: "/tmp"
- **java_home**: JAVA_HOME directory. Default: "{{install_dir}}/jdk{{version}}"
- **user**: Installation owner username. It must exists on the remote machine. Default: "oracle"
- **group**: Installation owner group name. It must exists on the remote machine. Default: "oracle"
- **repository_server**: HTTP repository server that hosts Java JDK binaries.
- **repository_server_url**: Repository server URL.. Default: "http://{{repository_server}}:8080"
- **repository_server_context**: Repository server context for Java JDK binaries. Default: "/java/jdk/{{platform}}/{{jvm_type}}/{{jdk_filename}}"

Dependencies
------------

N/A

Example Playbook
----------------

```yaml
- name: Install Java JDK.
  hosts: myRemoteHost
  roles:
    - role: jdk_install
      vars:
        ansible_python_interpreter: auto
        ansible_interpreter_python_fallback: ['/usr/bin/python3','/usr/bin/python2','/usr/bin/python']
        process: install
        platform: solaris
        jvm_type: "hotspot"
        version: "1.8.0_321"
        install_dir: "/tmp"
        user: oracle
        group: oracle
        repository_server: "myHTTPRepoServer"
      tags: jdk_install
```

Example output
----------------

```
PLAY [Install Java JDK] ******************************************************************************************************************************************

TASK [Gathering Facts ] ******************************************************************************************************************************************
ok: [myRemoteHost]

TASK [Install JDK name={{ lookup('env', 'NODECONF_HOME')}}/roles/jdk_install] ************************************************************************************

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install : Validating arguments against arg spec 'main' - This role installs Java JDK in a remote machine argument_spec={'process': {'type': 'str', 'required': False, 'description': "Process to execute ['install','deinstall']. You can choose install Java JDK or deintall it.", 'choices': ['install', 'deinstall'], 'default': ['install']}, 'platform': {'type': 'str', 'required': False, 'description': "Destination machine OS platform. Used in conjunction with 'jvm_type' and 'version' variables to build the context and get binaries from HTTP repository server.", 'default': 'solaris'}, 'jvm_type': {'type': 'str', 'required': False, 'description': "JVM type. Example: 'hotspot', 'openjdk'.", 'default': 'hotspot'}, 'version': {'type': 'str', 'required': False, 'description': 'JDK version.', 'default': '1.8.0_321'}, 'install_dir': {'type': 'str', 'required': False, 'description': 'Installation directory. JDK binaries are unarchived under this directory.', 'default': '/tmp'}, 'java_home': {'type': 'str', 'required': False, 'description': 'JAVA_HOME directory.', 'default': '/tmp/jdk1.8.0_321'}, 'user': {'type': 'str', 'required': False, 'description': 'Installation owner username. It must exists on the remote machine.', 'default': 'oracle'}, 'group': {'type': 'str', 'required': False, 'description': 'Installation owner group name. It must exists on the remote machine.', 'default': 'oracle'}, 'repository_server': {'type': 'str', 'required': False, 'description': 'HTTP repository server that hosts Java JDK binaries.'}, 'repository_server_url': {'type': 'str', 'required': False, 'description': 'Repository server URL.', 'default': 'http://myHTTPRepoServer:8080'}, 'repository_server_context': {'type': 'str', 'required': False, 'description': 'Repository server context for Java JDK binaries.', 'default': '/java/jdk/solaris/hotspot/jdk1.8.0_321.tar.gz'}}, provided_arguments={}, validate_args_context={'type': 'role', 'name': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install', 'argument_spec_name': 'main', 'path': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install'}] ***
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install : stat java_home] ***************************************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install : Check if binary is present in repository url=http://myHTTPRepoServer:8080/java/jdk/solaris/hotspot/jdk1.8.0_321.tar.gz, method=HEAD, status_code=200] ***
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install : Create tmp directory] *********************************************************
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install : Download from HTTP server test] ***********************************************
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install : Creates /tmp directory if not exists] *****************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install : Installing JDK in /tmp/jdk1.8.0_321 src=/tmp/jdk_install-9_v_vO/jdk1.8.0_321.tar.gz, dest=/tmp, remote_src=True, creates=/tmp/jdk1.8.0_321/bin/java, owner=oracle, group=oracle] ***
changed: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/jdk_install : Remove tmp dir] ***************************************************************
changed: [myRemoteHost]

PLAY RECAP *******************************************************************************************************************************************************
myRemoteHost              : ok=9    changed=4    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0   
```

License
-------

BSD

Author Information
------------------

Daniel Benítez Águila
