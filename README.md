# Ansible role to install and configure Jitsi Meet

[![Build Status](https://travis-ci.com/systemli/ansible-role-jitsi-meet.svg?branch=master)](https://travis-ci.com/systemli/ansible-role-jitsi-meet) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-jitsi_meet-blue.svg)](https://galaxy.ansible.com/systemli/jitsi_meet/)

This role installs and configure [Jitsi Meet](https://jitsi.org/jitsi-meet/) with nginx Webserver and prosody as XMPP Server.

Role Variables
--------------

```
---
jitsi_meet_server_name: "meet.example.com"
jitsi_meet_videobridge_loglevel: "ERROR"
jitsi_meet_videobridge_secret: "CHANGEME"
jitsi_meet_videobridge_port: 5347
jitsi_meet_jicofo_loglevel: "ERROR"
jitsi_meet_jicofo_user: focus
jitsi_meet_jicofo_secret: "CHANGEME2"
jitsi_meet_jicofo_port: 5347
jitsi_meet_jicofo_password: "CHANGEME3"

jitsi_meet_cert_choice: "Generate a new self-signed certificate (You will later get a chance to obtain a Let's encrypt certificate)"
jitsi_meet_ssl_cert_path: "/etc/ssl/certs/ssl-cert-snakeoil.pem"
jitsi_meet_ssl_key_path: "/etc/ssl/private/ssl-cert-snakeoil.key"

jitsi_meet_debconf_settings:
  - name: jitsi-meet
    question: jitsi-meet/cert-choice
    value: "{{ jitsi_meet_cert_choice }}"
    vtype: string
  - name: jitsi-meet
    question: jitsi-meet/jvb-serve
    value: "true"
    vtype: boolean
  - name: jitsi-meet-prosody
    question: jitsi-meet-prosody/jvb-hostname
    value: "{{ jitsi_meet_server_name }}"
    vtype: string
  - name: jitsi-videobridge
    question: jitsi-videobridge/jvb-hostname
    value: "{{ jitsi_meet_server_name }}"
    vtype: string
jitsi_meet_config_resolution: 720
jitsi_meet_config_disable_third_party_requests: "true"
jitsi_meet_config_p2p_enabled: "true"
# For privacy reasons we recommend to use your own STUN servers 
jitsi_meet_config_stun_servers:
  - stun.l.google.com:19302
  - stun1.l.google.com:19302
  - stun2.l.google.com:19302
jitsi_meet_config_default_language: en
jitsi_meet_config_last_n: "-1"

```

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
