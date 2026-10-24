# Ansible role to install and configure Jitsi Meet

[![Build Status](https://github.com/systemli/ansible-role-jitsi-meet/workflows/Integration/badge.svg?branch=main)](https://github.com/systemli/ansible-role-jitsi-meet/actions?query=workflow%3AIntegration)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-jitsi_meet-blue.svg)](https://galaxy.ansible.com/systemli/jitsi_meet/)

This role installs and configure [Jitsi Meet](https://jitsi.org/jitsi-meet/) with nginx Webserver and prosody as XMPP Server.

It supports Debian 12 (bookworm) and Debian 13 (trixie), which are also the
distributions tested in CI.

## Role Variables

You need to set the `jitsi_meet_base_secret` variable from which passwords for
the various internally used accounts are created.

See [`defaults/main.yml`](defaults/main.yml) for further available variables.

## Dependencies

- [systemli.apt_repositories](https://galaxy.ansible.com/systemli/apt_repositories)

## Download

Download latest release with `ansible-galaxy`

```shell
ansible-galaxy install systemli.jitsi_meet
```

## Example Playbook

```yaml
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

## Caveats

A change of the `jitsi_meet_server_name` variable applied on an already
deployed instance is going to break the configuration.

## Tests

We do integration tests with GitHub Actions and Molecule.

Locally, you can test the role with the Docker driver of Molecule:

```shell
pip install molecule "molecule-plugins[docker]" ansible-lint yamllint
molecule test
```

This requires a running Docker (or compatible) container runtime.

## Debugging

For local debugging you can run the following commands:

```shell
molecule converge
molecule login
```

This spins up a Debian 13 container, applies the role and opens a shell in it.

## Remove Jitsi

```shell
sudo apt purge jigasi jitsi-meet jitsi-meet-web-config jitsi-meet-prosody jitsi-meet-turnserver jitsi-meet-web jicofo jitsi-videobridge2 nginx* jitsi* prosody lua5.2
sudo apt autoremove --purge
sudo rm -r /etc/jitsi/ /usr/share/jitsi-meet/ /usr/share/jitsi-videobridge/ /etc/prosody/ /var/lib/prosody/ /etc/apt/sources.list.d/download_jitsi_org.sources /etc/apt/sources.list.d/packages.prosody.im.sources /etc/apt/preferences.d/download_jitsi_org.pref /etc/apt/preferences.d/packages.prosody.im.pref
```

## License

GPLv3

## Author Information

<https://www.systemli.org>
