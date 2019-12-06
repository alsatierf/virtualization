# Virtualization

This is an Ansible project which aims to install [Virtualbox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com/) locally.

## Dependencies

* MacOS X
* [Ansible](https://docs.ansible.com/ansible/latest/index.html)
* [Git](https://git-scm.com/)
* Privileged access (*i.e.* **sudo**)

## Execution

```bash
$ git clone https://github.com/alsfreitaz/virtualization.git
$ cd virtualization
$ ansible-playbook -i inventory.yml playbook.yml --ask-become-pass
```

## Description

This module will install Vagrant and Virtualbox on the machine where ansible-playbook is called from (*i.e.* localhost).

Default versions for each of these applications are already provided by the Ansible role default variables [vagrant_version](https://github.com/alsfreitaz/virtualization/blob/4897f043b1a187eb65bef82520967e828a0cdd7c/roles/vagrant/defaults/main.yml#L11) and [virtualbox_version](https://github.com/alsfreitaz/virtualization/blob/4897f043b1a187eb65bef82520967e828a0cdd7c/roles/virtualbox/defaults/main.yml#L15). As with any other Ansible project, the default role variables can be easily overridden if needed (please refer to the official documentation on [using variables](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html)).

On MacOS X systems, the main playbook relies on the existence of [Homebrew](https://brew.sh/) to install other the applications using cask so, if not present, it will be downloaded and installed by Ansible beforehand.

## Variables

### Vagrant Role

The [vagrant](https://github.com/alsfreitaz/virtualization/tree/master/roles/vagrant) role depends on the existence of the following variables (see all [current default values](https://github.com/alsfreitaz/virtualization/blob/master/roles/vagrant/defaults/main.yml)):

```yaml
vagrant:
  ANSIBLE_OS_FAMILY:
    versions:
      VERSION_X: 
        cask_file: CASK_TEMPLATE
    default_version: VERSION_Y
    
vagrant_version: VERSION_Z
```

For MacOS systems:

> `ANSIBLE_OS_FAMILY` must be set to `"Darwin"`. `VERSION_X` is a key to uniquely identify the cask template file used to install a pinned version of Vagrant. When `default_version = VERSION_Y` and `VERSION_Y == VERSION_X`, `VERSION_X` will be installed **if and only if** variable `vagrant_version` is not set. If `vagrant_version` refers to a specific version `VERSION_X` then the corresponding template file `CASK_TEMPLATE` will be used despite the value of `default_version`.

### Virtualbox Role

The [virtualbox](https://github.com/alsfreitaz/virtualization/tree/master/roles/virtualbox) role depends on the existence of the following variables (see all [current default values](https://github.com/alsfreitaz/virtualization/blob/master/roles/virtualbox/defaults/main.yml)):

```yaml
virtualbox:
  ANSIBLE_OS_FAMILY:
    versions:
      VERSION_X: 
        cask_file: CASK_TEMPLATE
        extensions:
          filename: FILENAME
          url: URL
          checksum: CHECKSUM 
    default_version: VERSION_Y

virtualbox_version: VERSION_Z
```

For MacOS X systems:

> `ANSIBLE_OS_FAMILY` must be set to `"Darwin"`. `VERSION_X` is a key to uniquely identify the cask template file used to install a pinned version of Virtualbox. When `default_version = VERSION_Y` and `VERSION_Y == VERSION_X`, `VERSION_X` will be installed **if and only if** variable `virtualbox_version` is not set. If `virtualbox_version` refers to a specific version `VERSION_X` then the corresponding template file `CASK_TEMPLATE` will be used despite the value of `default_version`.

> Variables `filename`, `url` and `checksum` are tighly related to a unique matching version of Virtualbox Extension Pack which, in turn, must be related to the specific version of Virtualbox described in `CASK_TEMPLATE`. The values `FILENAME`, `URL` and `CHECKSUM` for a given Virtualbox version can be found in the projects' [downloads page](https://www.virtualbox.org/wiki/Downloads).

## Supported Systems

Currently, this playbook works only on MacOS X systems and have been tested against the following configurations:

* Ansible 2.9.1
* Python 3.7.5 (used by Ansible)
* macOS 10.13.6 (kernel Darwin 17.7.0)
