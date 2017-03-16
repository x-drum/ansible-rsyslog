ansible-rsyslog
=========

This role installs and configures rsyslog.

Supported Platforms
-------------------

* RHEL 5/6/7
* Archlinux
* Ubuntu Trusty/Xenial
* Debian Wheezy

It will likely run on other platforms, just drop in vars/ a new file to support your os variant, vars are parsed in the following order/format:
* {{ ansible_distribution }}_{{ ansible_distribution_major_version }}.yml
* {{ ansible_distribution }}.yml
* {{ ansible_os_family }}_{{ ansible_distribution_major_version }}.yml
* {{ ansible_os_family }}.yml"

Requirements
------------

None at this time.

Default Role Variables
--------------

By default this role will install rsyslog and provide a minimal configuration, however variables can be passed to this role
and configuration can be overridden, for additional informations please have a look to **defaults/main.yml**


**rsyslog_default_config**: Setup a default basic configuration stanza (default: "True")  
**purge_config**: Purge existing config snippets (default: "False")  
**use_repo**: Use Adiscon rsyslog official package repository (default: "False")  
**repo_releasever**: Default rsyslog major release repository version to use (default: 8)  
**rsyslog_action_file_template**: Define a custom template for file logging (default: RSYSLOG_TraditionalFileFormat)  
**rsyslog_priv_drop_to_user**: Drop root privileges and switch to given user (default: root)  
**rsyslog_priv_drop_to_group**: Drop root privileges and switch to given group (default: root)  
**rsyslog_server_udp**: Enable a simple UDP server receiver (default: False)  
**rsyslog_server_udp_name**: Assign a name to the given receiver (default: "imudp")  
**rsyslog_server_udp_port**: Specifies the port the server shall listen to (default: "514")  
**rsyslog_server_udp_address**: Local ip address the udp server should listen (default: "0.0.0.0")  
**rsyslog_server_udp_ratelimit**: The rate-limiting interval in seconds (default: "5")  
**rsyslog_server_tcp**: Enable a simple TCP server receiver (default: False)  
**rsyslog_server_tcp_name**: Assign a name to the given receiver (default: "imtcp")  
**rsyslog_server_tcp_port**: Specifies the port the server shall listen to (default: "514")  
**rsyslog_server_tcp_address**: Local ip address the tcp server should listen **POSSIBLY BROKEN** (default: "0.0.0.0")  
**rsyslog_server_tcp_ratelimit**: The rate-limiting interval in seconds (default: "5")  

Additional Role Variables:
--------------
**rsyslog_custom_config**: Use a custom template to use as main configuration file (eg: rsyslog_custom_config: /path/to/rsyslog_custom.j2)  


Dependencies
------------

None at this time.

Example Playbook
----------------
1) Just install rsyslog with default configuration (it will be placed in /etc/rsyslog.d/)
```yaml
- hosts: all
  remote_user: root
  sudo: no
  - roles: 
    - { role: rsyslog }
```
2) Install rsyslog, without default configuration and setup two different custom stanzas
```yaml
- hosts: all
  remote_user: root
  vars:
    rsyslog_default_config: False
  roles:
  - role: rsyslog
    items:
    - name: "20-iptables"
      lines: 
        - ':msg, contains, "iptables" /var/log/iptables.log'
        - '& ~'
    - name: "30-dovecot"
      lines: 
        - 'if $programname == "dovecot" and $syslogseverity <= "6" then ~'
        - '& ~'
```
3) Install rsyslog, specify a custom configuration template
```yaml
- hosts: all
  remote_user: root
  vars:
    rsyslog_default_config: False
    rsyslog_custom_config: /home/servers/foo.bar/templates/rsyslog_custom.j2
  roles:
  - role: rsyslog
```

4) Install rsyslog using official repository packages, use major release 7
```
- hosts: all
  remote_user: root
  roles:
    - { role: rsyslog, "use_repo": True, "repo_releasever": 7 }
```

5) Enable a simple rsyslog UDP server (receiver) for remote logging
```yaml
- hosts: all
  vars:
  roles:
  - role: rsyslog
    rsyslog_server_udp_port: 514
    rsyslog_server_udp_address: 192.168.200.201
```

License
-------

GPLv2

Author Information
------------------

Alessio Cassibba (x-drum) http://blog.zerodev.it.
