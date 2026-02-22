# [Ansible role mediawiki](#ansible-role-mediawiki)

Install and configure mediawiki on your system.

|GitHub|GitLab|Downloads|Version|
|------|------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-mediawiki/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-mediawiki/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-mediawiki/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-mediawiki)|[![downloads](https://img.shields.io/ansible/role/d/buluma/mediawiki)](https://galaxy.ansible.com/buluma/mediawiki)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-mediawiki.svg)](https://github.com/buluma/ansible-role-mediawiki/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-mediawiki/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
- become: true
  gather_facts: true
  hosts: all
  name: Converge
  pre_tasks:
  - apt: update_cache=yes cache_valid_time=600
    changed_when: false
    name: Update apt cache.
    when: ansible_os_family == 'Debian'
  - ansible.builtin.stat:
      path: /usr/lib/python3.11/EXTERNALLY-MANAGED
    name: Check if python3.11 EXTERNALLY-MANAGED file exists
    register: externally_managed_file_py311
  - ansible.builtin.command:
      cmd: mv /usr/lib/python3.11/EXTERNALLY-MANAGED /usr/lib/python3.11/EXTERNALLY-MANAGED.old
    args:
      creates: /usr/lib/python3.11/EXTERNALLY-MANAGED.old
    name: Rename python3.11 EXTERNALLY-MANAGED file if it exists
    when: externally_managed_file_py311.stat.exists
  - ansible.builtin.stat:
      path: /usr/lib/python3.12/EXTERNALLY-MANAGED
    name: Check if python3.12 EXTERNALLY-MANAGED file exists
    register: externally_managed_file_py312
  - ansible.builtin.command:
      cmd: mv /usr/lib/python3.12/EXTERNALLY-MANAGED /usr/lib/python3.12/EXTERNALLY-MANAGED.old
    args:
      creates: /usr/lib/python3.12/EXTERNALLY-MANAGED.old
    name: Rename python3.12 EXTERNALLY-MANAGED file if it exists
    when: externally_managed_file_py312.stat.exists
  roles:
  - mediawiki_destination: /opt
    role: buluma.mediawiki
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-mediawiki/blob/master/molecule/default/prepare.yml):

```yaml
- become: true
  gather_facts: false
  hosts: all
  name: Prepare
  roles:
  - role: buluma.bootstrap
  - role: buluma.core_dependencies
  - role: buluma.epel
  - role: buluma.python_pip
  - role: buluma.buildtools
  - openssl_items:
    - common_name: '{{ ansible_fqdn }}'
      name: apache-httpd
    role: buluma.openssl
  - role: buluma.httpd
  - role: buluma.php
  - mysql_databases:
    - name: mediawiki
    mysql_users:
    - name: mediawiki
      password: m3d14w1k1
      priv: mediawiki.*:ALL
    role: buluma.mysql
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-mediawiki/blob/master/defaults/main.yml):

```yaml
mediawiki_destination: '{{ _mediawiki_destination[ansible_distribution] | default(_mediawiki_destination[''default''])
  }}'
mediawiki_major: 1
mediawiki_minor: 37
mediawiki_release: 1
mediawiki_version: '{{ mediawiki_major }}.{{ mediawiki_minor }}.{{ mediawiki_release
  }}'
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-mediawiki/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Build Status GitHub](https://github.com/buluma/ansible-role-buildtools/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-buildtools/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-buildtools)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Build Status GitHub](https://github.com/buluma/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-core_dependencies/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Build Status GitHub](https://github.com/buluma/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-epel/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-epel/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-epel)|
|[buluma.httpd](https://galaxy.ansible.com/buluma/httpd)|[![Build Status GitHub](https://github.com/buluma/ansible-role-httpd/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-httpd/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-httpd/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-httpd)|
|[buluma.mysql](https://galaxy.ansible.com/buluma/mysql)|[![Build Status GitHub](https://github.com/buluma/ansible-role-mysql/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-mysql/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-mysql/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-mysql)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Build Status GitHub](https://github.com/buluma/ansible-role-openssl/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-openssl/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-openssl)|
|[buluma.php](https://galaxy.ansible.com/buluma/php)|[![Build Status GitHub](https://github.com/buluma/ansible-role-php/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-php/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-php/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-php)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Build Status GitHub](https://github.com/buluma/ansible-role-python_pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-python_pip/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-python_pip)|

## [Dependencies](#dependencies)

Most roles require some kind of preparation, this is done in `molecule/default/prepare.yml`. This role has a "hard" dependency on the following roles:

- {'role': 'buluma.httpd'}

## [Context](#context)

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-mediawiki/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|

The minimum version of Ansible required is 2.12, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-mediawiki/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-mediawiki/blob/master/LICENSE).

## [Author Information](#author-information)

[Michael Buluma](https://buluma.github.io/)

