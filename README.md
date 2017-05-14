Ansible Role Fluentd
====================

An ansible role for installing td-agent (the EL packaged version of fluentd) version >= 2.3.4 with a basic config.

Requirements
------------

None.

Role Variables
--------------

Variable                  |Default                            | Description
--------------------------|:---------------------------------:|-------------
tdagent_version           |`2.3.5`                            |The td-agent version to install (must be >= 2.3.4)
tdagent_rpm               |`"td-agent-{{ tdagent_version }}"` |The td-agent rpm to install
tdagent_tcp_in_port       |`24224`                            |The input port for TCP, used by log forwarding and the fluent-cat
tdagent_http_in_port      |`9880`                             |The input port for HTTP POST
tdagent_enable_monitoring |`true`                             |Whether to enable the internal metrics
tdagent_monitor_port      |`24220`                            |Port to serve the internal metrics on as JSON
tdagent_enable_debug      |`true`                             |Whether to enable the debug agent
tdagent_debug_port        |`24230`                            |Port for debug agent to listen on
tdagent_plugins           |`[]`                               |List of fluentd plugins to install in the form `[ { name: "plugin-name", version: "plugin-version" } ]`

Dependencies
------------

None.

Example Requirements File
-------------------------

```
- src: https://github.com/companieshouse/ansible-role-fluentd
  name: fluentd
```

Example Playbook
----------------
```
    - hosts: servers
      roles:
         - role: fluentd
           tdagent_version: 2.3.5
           tdagent_plugins:
             - name: plugin-name
               version: 1.0.0
```

Custom Configs
--------------

Custom config files (`*.conf`) added to `/etc/td-agent/conf.d/` will be included by the basic config installed by this role.  After installing custom configs, `td-agent` must be reloaded.  If you are installing your configs in another ansible role (recommended) then you should use `notify: reload td-agent` to load your new configs.

License
-------

MIT
