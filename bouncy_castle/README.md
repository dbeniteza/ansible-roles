bouncy_castle
=========

The role `bouncy_castle` installs Bouncy Castle libraries in a remote machine with a JDK installation.

Requirements
------------

The role requires a existing Java JDK installation in the remote machine. 
It may require a HTTP repository server to download the libraries from. Set server details in `repository_xxx` variables (check bouncy_castle/defaults/main.yml).
If no HTTP repository server is available, you can set `download_method='direct'` to direct download libraries from Bouncy Castle official website.

Role Variables
--------------

- **download_method**: Bouncy Castle libraries download method ['repository','direct']. You can choose download from a local repository or directly from Internet. Default: "repository"
- **java_home**: JAVA_HOME directory. Default: "/tmp/jdk"
- **user**: Installation owner username. It must exists on the remote machine. Default: "oracle"
- **group**: Installation owner group name. It must exists on the remote machine. Default: "oracle"
- **jdk_version_jar**: JDK version jar. Bouncy Castle jar file names vary depending on the JDK version ('JDK 1.5 - JDK 1.8' or 'JDK 1.8 and later'). Default: "jdk18on"
- **repository_server**: HTTP repository server that hosts Bouncy Castle libraries.
- **repository_server_download_url**: Repository server URL to download Bouncy Castle libraries.
- **bc_download_url**: Bouncy Castle official website to download Bouncy Castle libraries. Default: "https://www.bouncycastle.org/download"
- **download_url**: URL used to download libraries. It depends on 'download_method' variable value.
- **bc_version**: Bouncy Castle libraries latest version. Default: "172"
- **bc_jars**: List of Bouncy Castle jar files.

Dependencies
------------

N/A

Example Playbook
----------------

```yaml
- name: Install Bouncy Castle
  hosts: myRemoteHost
  roles:
    - role: bouncy_castle
      vars:
        ansible_python_interpreter: auto
        ansible_interpreter_python_fallback: ['/usr/bin/python3','/usr/bin/python2','/usr/bin/python']
        download_method: "repository"
        java_home: "/tmp/jdk1.8.0_271"
        jdk_version_jar: "jdk18on"
        user: oracle
        group: oracle
        repository_server: "myHTTPRepoServer"
      tags: bouncy_castle
```

Example output
----------------

```
PLAY [Install Bouncy Castle] ****************************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts ] *********************************************************************************************************************************************************************************************************************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Validating arguments against arg spec 'main' - This role installs Bouncy Castle libraries in a remote machine with a JDK installation. argument_spec={'download_method': {'type': 'str', 'required': False, 'description': "Bouncy Castle libraries download method ['repository','direct']. You can choose download from a local repository or directly from Internet.", 'choices': ['repository', 'direct'], 'default': ['repository']}, 'java_home': {'type': 'str', 'required': False, 'description': 'JAVA_HOME directory.', 'default': '/tmp/jdk'}, 'user': {'type': 'str', 'required': False, 'description': 'Installation owner username. It must exists on the remote machine.', 'default': 'oracle'}, 'group': {'type': 'str', 'required': False, 'description': 'Installation owner group name. It must exists on the remote machine.', 'default': 'oracle'}, 'jdk_version_jar': {'type': 'str', 'required': False, 'description': "JDK version jar. Bouncy Castle jar file names vary depending on the JDK version ('JDK 1.5 - JDK 1.8' or 'JDK 1.8 and later').", 'choices': ['jdk18on', 'jdk15to18'], 'default': 'jdk18on'}, 'repository_server': {'type': 'str', 'required': False, 'description': 'HTTP repository server that hosts Bouncy Castle libraries.'}, 'repository_server_download_url': {'type': 'str', 'required': False, 'description': 'Repository server URL to download Bouncy Castle libraries.'}, 'bc_download_url': {'type': 'str', 'required': False, 'description': 'Bouncy Castle official website to download Bouncy Castle libraries.', 'default': 'https://www.bouncycastle.org/download'}, 'download_url': {'type': 'str', 'required': False, 'description': "URL used to download libraries. It depends on 'download_method' variable value."}, 'bc_version': {'type': 'str', 'required': False, 'description': 'Bouncy Castle libraries latest version.', 'default': '172'}, 'bc_jars': {'type': 'str', 'required': False, 'description': 'List of Bouncy Castle jar files.'}}, provided_arguments={}, validate_args_context={'type': 'role', 'name': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle', 'argument_spec_name': 'main', 'path': '/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle'}] ***
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Check java version installed in /tmp/jdk1.8.0_271 _raw_params=//tmp/jdk1.8.0_271/bin/java -version 2>&1 | grep version | awk '{print $3}' | sed 's/"//g', _uses_shell=True, _ansible_check_mode=False, _ansible_no_log=False, _ansible_debug=False, _ansible_diff=False, _ansible_verbosity=0, _ansible_version=2.13.6, _ansible_module_name=ansible.legacy.command, _ansible_syslog_facility=LOG_USER, _ansible_selinux_special_fs=['fuse', 'nfs', 'vboxsf', 'ramfs', '9p', 'vfat'], _ansible_string_conversion_action=warn, _ansible_socket=None, _ansible_shell_executable=/bin/sh, _ansible_keep_remote_files=False, _ansible_tmpdir=/root/.ansible/tmp/ansible-tmp-1671449793.7531161-128684-105686840112083/, _ansible_remote_tmp=$HOME/.ansible/tmp] ***
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : ansible.builtin.debug msg=Java version installed: 1.8.0_271] *******************************************************************************************************************
ok: [myRemoteHost] => {
    "msg": "Java version installed: 1.8.0_271"
}

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Check if Bouncy Castle is installed] *******************************************************************************************************************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : ansible.builtin.debug msg=Bouncy Castle is not installed] **********************************************************************************************************************
ok: [myRemoteHost] => {
    "msg": "Bouncy Castle is not installed"
}

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Set repository context based on java version jdk_version_jar=jdk18on] **********************************************************************************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Check if jars are present url=http://myHTTPRepoServer:8080/java/bouncy_castle/latest//bcjmail-jdk18on-172.jar, method=HEAD, status_code=200] *************************
ok: [myRemoteHost -> localhost] => (item=bcjmail-jdk18on-172.jar)
ok: [myRemoteHost -> localhost] => (item=bcmail-jdk18on-172.jar)
ok: [myRemoteHost -> localhost] => (item=bcpg-jdk18on-172.jar)
ok: [myRemoteHost -> localhost] => (item=bcpkix-jdk18on-172.jar)
ok: [myRemoteHost -> localhost] => (item=bcprov-ext-jdk18on-172.jar)
ok: [myRemoteHost -> localhost] => (item=bcprov-jdk18on-172.jar)
ok: [myRemoteHost -> localhost] => (item=bctls-jdk18on-172.jar)
ok: [myRemoteHost -> localhost] => (item=bcutil-jdk18on-172.jar)

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Download Bouncy Castle jar files from http://myHTTPRepoServer:8080/java/bouncy_castle/latest/ url=http://myHTTPRepoServer:8080/java/bouncy_castle/latest//bcjmail-jdk18on-172.jar, dest=/tmp/jdk1.8.0_271/jre/lib/ext, mode=0644, owner=oracle, group=oracle, timeout=30, _ansible_check_mode=False, _ansible_no_log=False, _ansible_debug=False, _ansible_diff=False, _ansible_verbosity=0, _ansible_version=2.13.6, _ansible_module_name=ansible.builtin.get_url, _ansible_syslog_facility=LOG_USER, _ansible_selinux_special_fs=['fuse', 'nfs', 'vboxsf', 'ramfs', '9p', 'vfat'], _ansible_string_conversion_action=warn, _ansible_socket=None, _ansible_shell_executable=/bin/sh, _ansible_keep_remote_files=False, _ansible_tmpdir=/root/.ansible/tmp/ansible-tmp-1671449799.7832465-128900-228891773446177/, _ansible_remote_tmp=$HOME/.ansible/tmp] ***
changed: [myRemoteHost] => (item=bcjmail-jdk18on-172.jar)
changed: [myRemoteHost] => (item=bcmail-jdk18on-172.jar)
changed: [myRemoteHost] => (item=bcpg-jdk18on-172.jar)
changed: [myRemoteHost] => (item=bcpkix-jdk18on-172.jar)
changed: [myRemoteHost] => (item=bcprov-ext-jdk18on-172.jar)
changed: [myRemoteHost] => (item=bcprov-jdk18on-172.jar)
changed: [myRemoteHost] => (item=bctls-jdk18on-172.jar)
changed: [myRemoteHost] => (item=bcutil-jdk18on-172.jar)

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Read /tmp/jdk1.8.0_271/jre/lib/security/java.security] *************************************************************************************************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Get last provider number] ******************************************************************************************************************************************************
ok: [myRemoteHost]

TASK [/home/usr-a.dbenitez/repos/arqtooling/playbooks/nodeconf/roles/bouncy_castle : Add BouncyCastleProvider to /tmp/jdk1.8.0_271/jre/lib/security/java.security] **************************************************************************************************
changed: [myRemoteHost]

PLAY RECAP **********************************************************************************************************************************************************************************************************************************************************
myRemoteHost              : ok=12   changed=2    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
```

License
-------

BSD

Author Information
------------------

Daniel Benítez Águila
