# [minikube](#minikube)

Install and configure minikube on your system.

|Travis|GitHub|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-minikube.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-minikube)|[![github](https://github.com/robertdebock/ansible-role-minikube/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-minikube/actions)|[![quality](https://img.shields.io/ansible/quality/42933)](https://galaxy.ansible.com/robertdebock/minikube)|[![downloads](https://img.shields.io/ansible/role/d/42933)](https://galaxy.ansible.com/robertdebock/minikube)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-minikube.svg)](https://github.com/robertdebock/ansible-role-minikube/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.minikube
      minikube_user: minikube
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.core_dependencies
    - role: robertdebock.epel
    - role: robertdebock.python_pip
    - role: robertdebock.docker
    - role: robertdebock.kubectl
    - role: robertdebock.sysctl
      sysctl_items:
        - name: net.bridge.bridge-nf-call-iptables
          value: 1
        - name: fs.protected_regular
          value: 0
    - role: robertdebock.users
      users_user_list:
        - name: minikube
          groups: docker
```

For verification `molecule/resources/verify.yml` runs after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: no

  tasks:
    - name: run minikube --help
      command: /usr/bin/minikube --help
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for minikube

minikube_version: 1.11.0-0

# `minikube start` should start as a non-root-user. This should be an exising
# user on the Linux system. (Hint: robertdebock.users)
minikube_user: minikube
```

## [Requirements](#requirements)

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.core_dependencies
- robertdebock.docker
- robertdebock.epel
- robertdebock.kubectl
- robertdebock.python_pip
- robertdebock.service
- robertdebock.sysctl
- robertdebock.users

```

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/minikube.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|el|7, 8|
|debian|buster, bullseye|
|fedora|31, 32|
|ubuntu|focal, bionic, xenial|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.

## [Exceptions](#exceptions)

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| CentOS 6 | Depending role (python_pip) does not work on CentOS 6. |

## [Included version(s)](#included-versions)

This role [refers to a version](https://github.com/robertdebock/ansible-role-minikube/blob/master/defaults/main.yml) released by Google. Check the released version(s) here:
- [A Debian package of Minikube](https://storage.googleapis.com/minikube/releases).
- [A Red Hat package of Minikube](https://storage.googleapis.com/minikube/releases).

This version reference means a role may get outdated. Monthly tests occur to see if [bit-rot](https://en.wikipedia.org/wiki/Software_rot) occured. If you however find a problem, please create an issue, I'll get on it as soon as possible.
## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-minikube) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-minikube/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0


## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
