# Ansible role to install and configure Jitsi Meet

[![Build Status](https://travis-ci.com/systemli/ansible-role-jitsi-meet.svg?branch=master)](https://travis-ci.com/systemli/ansible-role-jitsi-meet) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-jitsi_meet-blue.svg)](https://galaxy.ansible.com/systemli/jitsi_meet/)

This role installs and configure [Jitsi Meet](https://jitsi.org/jitsi-meet/) with nginx Webserver and prosody as XMPP Server.

It is maintained with Debian Buster in mind, but should also work with its
derivatives like Ubuntu 20.04.


Role Variables
--------------

You need to set the `jitsi_meet_base_secret` variable from which passwords for
the various internally used accounts are created.

See [`defaults/main.yml`](defaults/main.yml) for further available variables.

Dependencies
------------

 - [systemli.apt_repositories](https://galaxy.ansible.com/systemli/apt_repositories)


Download
--------

Download latest release with `ansible-galaxy`

	ansible-galaxy install systemli.jitsi_meet

Example Playbook
----------------

```
- hosts: jitsimeetservers
  roles:
     - { role: systemli.letsencrypt }
     - { role: systemli.jitsi_meet }
  vars:
    jitsi_meet_server_name: "meet.example.com"
    letsencrypt_cert:
      name: "{{ jitsi_meet_server_name }}"
      domains:
        - "{{ jitsi_meet_server_name }}"
      challenge: dns
```

Caveats
-------

A change of the `jitsi_meet_server_name` variable applied on an already
deployed instance is going to break the configuration.

Tests
-----

For developing and testing the role we use Travis CI, Molecule and Vagrant. On the local environment you can easily test the role with

```
pip install molecule-vagrant ansible-lint yamllint
molecule test
```

This requires [Vagrant](https://www.vagrantup.com/downloads.html) to be installed.

License
-------

GPLv3

Author Information
------------------

https://www.systemli.org
