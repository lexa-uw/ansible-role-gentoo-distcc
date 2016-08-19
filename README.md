Role Name
=========

Ansible role which set up distcc in Gentoo. Also links within ccache if enabled.
Based on https://wiki.gentoo.org/wiki/Distcc

Requirements
------------

Works only on gentoo distro.
Correct working distcc servers with ccache.
```
(local)                  (remote)
ccache > distcc ==> distcc > ccache > hit?  > return
                                    > miss? > gcc -> cache & return
```

Role Variables
--------------

```yaml
distcc:
  ccache_config: /etc/ccache.conf       # path to ccache config. For linking with ccache
  cmdlist:                              # cmdlist for correct linking distcc server
    - /usr/lib/ccache/bin/g++           #
  dir: /var/tmp/portage/.distcc         # directory to store lock files and state files
  make_conf_MAKEOPTS_j: 25              # count of all cores
  make_conf_MAKEOPTS_l: 2               # count of local cores
  hosts: []                             # list of distcc servers
  remove_localhost: true                # remove localhos from list of distcc servers?
  server_options:                       # server options in /etc/conf.d/distcc
    - "--allow 192.168.1.0/24"          # if server options is empty, server will not be enabled
    - "--allow 127.0.0.1"
    - "--log-level info"
```

Example Playbook
----------------
```yaml
- hosts: servers
  roles:
     - { role: gentoo.distcc, hosts: [localhost, another-server] }
```

License
-------

BSD
