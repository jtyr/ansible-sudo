sudo
====

Role that helps to manage `/etc/sudoers` and `/etc/sudoers.d` through a set of
variables.


Example
-------

```
---

# Example of how to add ansible user into to /etc/sudoers.conf file
- hosts: myhost
  vars:
    sudo_users__custom:
      # Add a new definition for the ansible user to be able to run any command
      # without password
      - ansible:
          host: ALL
          runas: ALL
          tag: NOPASSWD
          cmd: ALL
  roles:
    - sudo

# Example of how to create sudoers.d file
- hosts: myhost
  vars:
    sudo_sudoersd:
      # This is the filename
      - my_file:
          # This is the users definition like above
          users:
            - ansible:
                host: ALL
                runas: ALL
                tag: NOPASSWD
                cmd: ALL
  roles:
    - sudo
```


Role variables
--------------

List of variables used by the role:

```
# Location of the main sudoers config file
sudo_sudoers_file: /etc/sudoers

# Location of the sudoers.d directory
sudo_sudoersd_dir: /etc/sudoers.d

# Whether to delete all unmanaged files
# (files not defined in the sudo_sudoersd variable)
sudo_sudoersd_managed: yes

# Deploy main /etc/sudoers.conf file
# (set to 'no' if only sudoers.d files should be created)
sudo_sudoers_deploy: yes

# Deploy main /etc/sudoers.conf file
# (set to 'no' if only the main sudoers file should be created)
sudo_sudoersd_deploy: yes

# Default path to the visudo
sudo_visudo: /usr/sbin/visudo

# Default host aliases
sudo_host_alias: {}
#sudo_host_alias:
#  FILESERVERS:
#    - fs1
#    - fs2
#  MAILSERVERS:
#    - smtp
#    - smtp2

# Default user aliases
sudo_user_alias: {}
#sudo_user_alias:
#  ADMINS:
#    - jsmith
#    - mikem
#  WEBMASTERS:
#    - peter
#    - lisa

# Default command aliases
sudo_cmd_alias: {}
#sudo_cmd_alias:
#  SOFTWARE
#    - /bin/rpm
#    - /usr/bin/up2date
#    - /usr/bin/yum
#  SERVICES:
#    - /sbin/service
#    - /sbin/chkconfig

# Default sudo defaults
sudo_defaults__default:
  - requiretty
  - '!visiblepw'
  - always_set_home
  - env_reset
  - env_keep  = "COLORS DISPLAY HOSTNAME HISTSIZE INPUTRC KDEDIR LS_COLORS"
  - env_keep += "MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE"
  - env_keep += "LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES"
  - env_keep += "LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE"
  - env_keep += "LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY"
  - secure_path = /sbin:/bin:/usr/sbin:/usr/bin

# Custom sudo defaults
sudo_defaults__custom: []

# Final sudo defaults
sudo_defaults: "{{
  sudo_defaults__default +
  sudo_defaults__custom
}}"

# Default sudo users
sudo_users__default:
  - root:
      host: ALL
      runas: ALL
      tag: ''
      cmd: ALL
#  - '%wheel':
#      host: ALL
#      runas: ALL
#      tag: NOPASSWD
#      cmd: ALL
#  - ADMINS:
#      host: ALL
#      runas: ''
#      tag: ''
#      cmd: ALL
#  - WEBMASTERS:
#      host: ALL
#      runas: ''
#      tag: ''
#      cmd: SOFTWARE, SERVICES

# Custom sudo users
sudo_users__custom: []

# Final sudo users
sudo_users: "{{
    sudo_users__default +
    sudo_users__custom
}}"

# Default file includes
sudo_include: []
#sudo_include:
#  - /etc/sudoers2

# Default directory includes
sudo_includedir:
  - /etc/sudoers.d

# Definition of files in /etc/sudoers.d
sudo_sudoersd: {}
#sudo_sudoersd:
#  my_file:
#    users:
#      - my_user:
#          host: ALL
#          runas: ALL
#          tag: NOPASSWD
#          cmd: /sbin/shutdown
```


License
-------

MIT


Author
------

Jiri Tyr
