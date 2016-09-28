ansible-rsyslog
=========

This role installs and configures rsyslog.

Supported Platforms
-------------------

* RHEL 6
* Archlinux
* Ubuntu Trusty
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
  sudo: no
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
3) Enable rsyslog server
```yaml
- hosts: all
  roles:
    - { role: ../../roles/ansible-rsyslog-custom, "rsyslog_server": yes }
```

License
-------

GPLv2

Author Information
------------------

Alessio Cassibba (x-drum) http://blog.zerodev.it.
