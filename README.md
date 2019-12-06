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

### Vagrant

The [vagrant](https://github.com/alsfreitaz/virtualization/tree/master/roles/vagrant) role depends on the existence of the following variables:

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

> `ANSIBLE_OS_FAMILY` must be set to `"Darwin"`. `VERSION_X` is a key to uniquely identify the cask template file to be used to install Vagrant and when `VERSION_Y == VERSION_X` the `VERSION_X` cask file will be installed **if** variable `vagrant_version` is not set. If `vagrant_version` is set to refer to a specific version `VERSION_X` then the cask_file associated with the corresponding template file `CASK_TEMPLATE` will be used.

### Virtualbox

The [virtualbox](https://github.com/alsfreitaz/virtualization/tree/master/roles/virtualbox) role depends on the existence of the following variables:

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

> `ANSIBLE_OS_FAMILY` must be set to `"Darwin"`. `VERSION_X` is a key to uniquely identify the cask template file to be used to install Virtualbox and when `VERSION_Y == VERSION_X` the `VERSION_X` cask file will be installed **if** variable `virtualbox_version` is not set. If `virtualbox_version` is set to refer to a specific version `VERSION_X` then the cask_file associated with the corresponding template file `CASK_TEMPLATE` will be used.

> The variables `FILENAME`, `URL` and `CHECKSUM` are related to a specific version of Virtualbox described in `CASK_TEMPLATE` and are used to install a matching Virtualbox Extension Pack. The corresponding filename, url and checksum for a given Virtualbox version can be found in the projects' [downloads page](https://www.virtualbox.org/wiki/Downloads).

## Supported Systems

Currently, this playbook works only on Mac OS X systems and have been tested against the following configurations:

* Ansible 2.9.1
* Python 3.7.5 (used by Ansible)
* macOS 10.13.6 (kernel Darwin 17.7.0)
