# Ansible role to install and configure Jitsi Meet

[![Build Status](https://github.com/systemli/ansible-role-jitsi-meet/workflows/Molecule/badge.svg?branch=master)](https://github.com/systemli/ansible-role-jitsi-meet/actions?query=workflow%3AIntegration)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-jitsi_meet-blue.svg)](https://galaxy.ansible.com/systemli/jitsi_meet/)

This role installs and configure [Jitsi Meet](https://jitsi.org/jitsi-meet/)
with nginx webserver and prosody as XMPP server.

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


Customization of the web application
------------------------------------

You can add additional and overriding asset files like CSS, images or included
HTML files by defining the variable `jitsi_meet_custom_assets_folder` that
would usually be located in a `files` folder that is a sibling to your playbook.

Have a look at the contents in `/usr/share/jitsi-meet/` of a deployed instance
for possible overridees. Some are meant to be included in the web application
like `static/welcomePageAdditionalContent.html`. A `title.html`, however,
would be overridden by this role.

CSS files from a `css` folder are automatically referenced in the application's
html.

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
