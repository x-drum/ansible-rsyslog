ansible-rsyslog
=========

This role installs and configures rsyslog.

Requirements
------------

None at this time.

Default Role Variables
--------------

By default this role will provide a minimal configuration

**rsyslog_main_config**: Main config file path (default: "/etc/rsyslog.conf")

**rsyslog_include_path**: Path of additional config stanzas (default: "/etc/rsyslog.d")

**rsyslog_file_mode**: Default mode for configuration files (default: "0640")

**rsyslog_file_create_mode**: Default mode for new files created by rsyslog  (default "0640")

**rsyslog_umask**: Specify the rsyslogd processes' umask (default "0022")

**rsyslog_abort_on_unclean_config**: Check config syntax on startup and abort if unclean (default: off)

**rsyslog_repeated_msg_reduction**: Reduce repeating messages (default: off)

**rsyslog_action_file_default_template**: Use the default, traditional logformat, as default for loggin (default: RSYSLOG_TraditionalFileFormat)

**rsyslog_action_file_template**: Define only when a custom logformat is neeed (default: undefined)

**rsyslog_priv_drop_to_user**: Name of the user rsyslog should run under after startup (default: undefined)

**rsyslog_priv_drop_to_group**: Name of the group rsyslog should run under after startup (default: undefined)

**rsyslog_default_config**: Setup a default basic configuration stanza (default: "True")


Dependencies
------------

None at this time.

Example Playbook
----------------

    - name: Apply rsyslog role
      remote_user: root
      hosts: all
      sudo: no
    - role: rsyslog
      items:
      - name: "20-iptables"
        lines: 
          - ':msg, contains, "iptables" /var/log/iptables.log'
          - '& ~'
      - name: "30-dovecot"
        lines: 
          - 'if $programname == 'dovecot' and $syslogseverity <= '6' then ~'
          - '& ~'

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

License
-------

GPLv2

Author Information
------------------

Alessio Cassibba (x-drum) http://blog.zerodev.it.
