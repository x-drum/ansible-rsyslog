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
**purge_config**: Purge existing config snippets (default: "False")
**use_repo**: Use Adiscon rsyslog official package repository (default: "False")
**repo_releasever**: Default rsyslog major release repository version to use (default: 8)
**rsyslog_action_file_template**: Define a custom template for file logging (default: RSYSLOG_TraditionalFileFormat)
**rsyslog_priv_drop_to_user**: Drop root privileges and switch to given user (default: root)
**rsyslog_priv_drop_to_group**: Drop root privileges and switch to given group (default: root)

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
3) Install rsyslog, and specify a custom configuration template
```yaml
- hosts: all
  remote_user: root
  sudo: no
  vars:
    rsyslog_default_config: False
    rsyslog_custom_config: /home/servers/foo.bar/templates/rsyslog_custom.j2
  roles:
  - role: rsyslog

```
4) Enable rsyslog server
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
