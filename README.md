<!-- get id via: ansible-galaxy info tehtbl.deployment_user | grep -i "id:" -->
<a href="https://galaxy.ansible.com/tehtbl/deployment_user"><img src="https://img.shields.io/ansible/role/453388"/></a> <a href="https://galaxy.ansible.com/tehtbl/deployment_user"><img src="https://img.shields.io/ansible/quality/453388"/></a> <a href="https://travis-ci.org/tehtbl/ansible-role-deployment_user"><img src="https://travis-ci.org/tehtbl/ansible-role-deployment_user.svg?branch=master" alt="Build status"/></a>

Role Description
================

Install and configure deployment user on a system.

Example Playbook
================

This example is taken from `molecule/default/playbook.yml` and is tested on each push, pull request and release.

```yaml
---
# ------------------------------------------------------------------------
# Install and configure deployment_user
# ------------------------------------------------------------------------
- name: deployment_user
  hosts: all
  become: true
  gather_facts: false

  roles:
    - role: tehtbl.bootstrap
    - role: tehtbl.deployment_user
      deployment_user_parameter: value

```

Role Variables
==============

These variables are set in `defaults/main.yml`:

```yaml
---
# ------------------------------------------------------------------------
# defaults file for deployment_user
# ------------------------------------------------------------------------

# Set deployment user
deployment_user_name: "{{ ansible_ssh_user }}"

# Set groups to be assigned deployment user for
deployment_user_grps:
  - admins

# Checks for password changes
deployment_user_check_new_pw: false
deployment_user_check_rnd_pwd: false

# Password for when deployment_user_check_new_pw == True
deployment_user_new_pw: "{{ 'password' | password_hash('sha512') }}"

# Sudo commands a deployment user can run
deployment_user_sudo_wo_password: false

# Sudo commands a deployment user can run
deployment_user_sudoers_commands:
  - /bin/dmesg
  - /usr/bin/apt update
  - /usr/bin/apt upgrade
  - /usr/bin/apt autoclean
  - /usr/bin/apt autoremove
  - /usr/bin/apt-get update
  - /usr/bin/apt-get upgrade
  - /usr/bin/apt-get autoclean
  - /usr/bin/apt-get autoremove
  - /usr/bin/lsof -Pni
  - /usr/bin/tail -f /var/log/syslog
  - /usr/sbin/ntpdate -uv *
  - /sbin/poweroff
  - /sbin/reboot

# Allowed SSH public key for user
deployment_user_pubkeys:
  - pubkey: ssh-rsa AAAAB3Nxxx== comment
    state: present

```

Requirements
============

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible (Tests run on the current, previous and next release of Ansible).

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- tehtbl.bootstrap
- tehtbl.ssh_server

```

Context
=======

This role is a part of many compatible roles. Have a look at [my other roles](https://github.com/tehtbl?utf8=%E2%9C%93&tab=repositories&q=ansible-role-&type=&language=) for further information.

Compatibility
=============

This role has been tested on these [Docker](https://hub.docker.com/) images:

|container|tag|allow_failures|
|---------|---|--------------|
|debian|stable|no|
|debian|testing|no|
|debian|unstable|yes|
|ubuntu|xenial|no|
|ubuntu|bionic|no|
|ubuntu|devel|yes|

This role has been tested on these Ansible versions:

- ansible>=2.8, <2.9
- ansible>=2.9
- git+https://github.com/ansible/ansible.git@devel

Testing Using Tox
=================

[Unit tests](https://travis-ci.org/tehtbl/ansible-role-deployment_user) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/tehtbl/ansible-role-deployment_user/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple Ansible versions. [Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed Ansible version, namespace: `tehtbl`, image: `ubuntu`, tag: `latest`):

```
molecule test

# Or select a specific image:
IMAGE="ubuntu" molecule test

# Or select a specific image and a specific tag:
IMAGE="debian" TAG="stable" tox
```

Or you can test multiple versions of Ansible, and select the correct images:

Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `tehtbl`, image: `ubuntu`, tag: `latest`) tests:

```
tox

# To run Ubuntu (namespace: `tehtbl`, tag: `latest`)
IMAGE="ubuntu" tox

# Or customize more:
IMAGE="debian" TAG="stable" tox -e py37-ansible-current
```

Testing Using Vagrant
=====================

Install `vagrant` plugins via:
```
vagrant plugin install vagrant-reload
```

Start Tests via *VirtualBox* Provider:
```
vagrant up
```

License
=======

GNU General Public License v3.0

Author Information
==================

<a href="https://github.com/tehtbl"><img src="https://img.shields.io/badge/GitHub-tehtbl-blue/?style=flat&logo=github" /></a> <a href="https://twitter.com/tehtbl"><img src="https://img.shields.io/badge/Twitter-tehtbl-blue/?style=flat&logo=twitter" /></a>

Sources
=======

This work is based on the great work of many people, e.g. [robertdebock](https://github.com/robertdebock) and [geerlingguy](https://github.com/geerlingguy). Thank you!
