# Ansible role [mediawiki](https://galaxy.ansible.com/ui/standalone/roles/buluma/mediawiki/documentation)

Install and configure mediawiki on your system.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-mediawiki/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-mediawiki/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-mediawiki.svg)](https://github.com/buluma/ansible-role-mediawiki/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-mediawiki.svg)](https://github.com/buluma/ansible-role-mediawiki/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-mediawiki.svg)](https://github.com/buluma/ansible-role-mediawiki/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/mediawiki)](https://galaxy.ansible.com/ui/standalone/roles/buluma/mediawiki/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-mediawiki/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: buluma.mediawiki
      mediawiki_destination: /opt
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-mediawiki/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: buluma.bootstrap
    - role: buluma.core_dependencies
    - role: buluma.epel
    - role: buluma.python_pip
    - role: buluma.buildtools
    - role: buluma.openssl
      openssl_items:
        - name: apache-httpd
          common_name: "{{ ansible_fqdn }}"
    - role: buluma.httpd
    - role: buluma.php
    - role: buluma.mysql
      mysql_databases:
        - name: mediawiki
      mysql_users:
        - name: mediawiki
          password: m3d14w1k1
          priv: "mediawiki.*:ALL"
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-mediawiki/blob/master/defaults/main.yml):

```yaml
---
# defaults file for mediawiki

# The version (major.minor.release) to install.
mediawiki_major: 1
mediawiki_minor: 37
mediawiki_release: 1

mediawiki_version: "{{ mediawiki_major }}.{{ mediawiki_minor }}.{{ mediawiki_release }}"

# Where to install mediawiki. You can use this pattern to install to a default
# location that differs per distribution, see `vars/main.yml`:
# "{{ _mediawiki_destination[ansible_distribution] | default(_mediawiki_destination['default'] ) }}"
# Change this to a simple string that refers to a path, for example:
# "/data/mediawiki".

mediawiki_destination: "{{ _mediawiki_destination[ansible_distribution] | default(_mediawiki_destination['default']) }}"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-mediawiki/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Ansible Molecule](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-buildtools.svg)](https://github.com/shadowwalker/ansible-role-buildtools)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Ansible Molecule](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-core_dependencies.svg)](https://github.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Ansible Molecule](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-epel/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-epel.svg)](https://github.com/shadowwalker/ansible-role-epel)|
|[buluma.httpd](https://galaxy.ansible.com/buluma/httpd)|[![Ansible Molecule](https://github.com/buluma/ansible-role-httpd/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-httpd/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-httpd.svg)](https://github.com/shadowwalker/ansible-role-httpd)|
|[buluma.mysql](https://galaxy.ansible.com/buluma/mysql)|[![Ansible Molecule](https://github.com/buluma/ansible-role-mysql/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-mysql/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-mysql.svg)](https://github.com/shadowwalker/ansible-role-mysql)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Ansible Molecule](https://github.com/buluma/ansible-role-openssl/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-openssl.svg)](https://github.com/shadowwalker/ansible-role-openssl)|
|[buluma.php](https://galaxy.ansible.com/buluma/php)|[![Ansible Molecule](https://github.com/buluma/ansible-role-php/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-php/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-php.svg)](https://github.com/shadowwalker/ansible-role-php)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Ansible Molecule](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-python_pip.svg)](https://github.com/shadowwalker/ansible-role-python_pip)|

## [Dependencies](#dependencies)

Most roles require some kind of preparation, this is done in `molecule/default/prepare.yml`. This role has a "hard" dependency on the following roles:

- {'role': 'buluma.httpd'}

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-mediawiki/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|all|
|[Kali](https://hub.docker.com/r/buluma/kali)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-mediawiki/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-mediawiki/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-mediawiki/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)

