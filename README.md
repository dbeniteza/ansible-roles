ansible-roles
=========

The repository `ansible-roles` contains Ansible roles that can help with infrastructure provisioning and automation.

# Roles

**bouncy_castle**
------------
The role `bouncy_castle` installs Bouncy Castle libraries in a remote machine with a JDK installation.

**jdk_install**
------------

The role `jdk_install` installs Java JDK.
First, it checks whether a JDK installation exists on the specified JAVA_HOME, then downloads JDK binaries from HTTP repository server to the remote machine. After that, it installs JDK on the JAVA_HOME directory.

**fmw_infra**
------------
The role `fmw_infra` installs Oracle Fusion Middleware Infrastructure in a remote machine with a JDK installation.

**oracledb_exporter**
------------

The role `oracledb_exporter` creates a container with Prometheus exporter image for Oracle database.
After adding this container as a target to Prometheus configuration (please check `prometheus_configuration` role), you can visualize collected metrics with specific Grafana dashboards available [here]( https://grafana.com/grafana/dashboards/3333-oracledb/) for Oracle database.

The container image is based on [iamseth/oracledb_exporter](https://github.com/iamseth/oracledb_exporter).
The container is named `<database>_oracledb_exporter`, being `<database>` the database instance name, and it listens on host port 9161 by default. Please check defaults (oracledb_exporter/defaults/main.yml) for further details.

**prometheus_configuration**
------------

The role `prometheus_configuration` configures Prometheus to add a container target that serves as exporter.
First, it checks whether a container exists with the specified name, then creates a block template in memory with the details of the target. After that, it updates Prometheus configuration file (prometheus.yaml) to include the block with the new target and finally reloads Prometheus configuration to visualize the target on the console.
