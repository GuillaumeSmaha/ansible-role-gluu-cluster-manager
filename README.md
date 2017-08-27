Ansible Gluu: Cluster Manager
==========

**gluu-cluster-manager** is an Ansible role to easily install and configure the Gluu Cluster Manager.

This role will configure the cluster manager and dispatch the SSH key to the gluus servers to allow configuration by the Gluu Cluster Manager.

To use the functionalities of the cluster mode in this role, all gluu servers have to be in the group `gluu-servers`
 and the Gluu Cluster Manager have to be in the group `gluu-cluster-manager`.


- [Requirements](#requirements)
- [Installation](#installation)
- [Update](#update)
- [Role Variables](#role-variables)
- [Deploying](#deploying)
- [Example Playbook](#example-playbook)
- [Sample projects](#sample-projects)

History
-------

Gluu's open source authentication & API access management server enables organizations to offer single sign-on, strong authentication, and centralize.

Requirements
------------

In order to deploy, you will need:

* Ansible in your deployer machine
* You also need to install this python dependency:
  - dnspython

```
$ pip install -r requirements.txt
```

Installation
------------

**gluu-cluster-manager** is an Ansible role distributed globally using [Ansible Galaxy](https://galaxy.ansible.com/). In order to install **gluu-cluster-manager** role you can use the following command.

```
$ ansible-galaxy install GuillaumeSmaha.gluu-cluster-manager
```


Update
------

If you want to update the role, you need to pass **--force** parameter when installing. Please, check the following command:

```
$ ansible-galaxy install --force GuillaumeSmaha.gluu-cluster-manager
```


Role Variables
--------------


```yaml
vars:

  # IP address of the host.
  gluu_ip: "{{ lookup('dig', '{{ ansible_ssh_host }}.')}}"

  # MySQL password for gluu user.
  gluu_cluster_mysql_user_password: "{{ lookup('pipe', \"cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 12 | head -n 1\") }}"

  # Gluu cluster application mode: `prod` or `test`.
  gluu_cluster_app_mode: prod

  # Gluu cluster manager secret.
  gluu_cluster_app_secret: "{{ lookup('pipe', \"cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1\") }}"
```

Deploying
---------

In order to deploy, you need to perform some steps:

* Create a new `hosts` file. Check [ansible inventory documentation](http://docs.ansible.com/intro_inventory.html) if you need help.
* Create a new playbook for deploying your app, for example, `deploy.yml`
* Set up role variables (see [Role Variables](#role-variables))
* Include the `GuillaumeSmaha.gluu-cluster-manager` role as part of a play
* Run the deployment playbook

```ansible-playbook -i hosts deploy.yml```

If everything has been set up properly, this command will install Gluu Cluster Manager on the host.


Example Playbook
----------------

In the folder, example you can check an example project that shows how to deploy.

In order to run it, you will need to have Vagrant and the role installed. Please check https://www.vagrantup.com for more information about Vagrant and our Installation section.

```
$ cd example
$ vagrant plugin install vagrant-lxc
$ vagrant plugin install vagrant-hostmanager
$ vagrant up --provider=lxc
$ ansible-galaxy install GuillaumeSmaha.gluu-cluster-manager
$ ansible-playbook -i hosts deploy.yml
$ ssh -L 6080:localhost:6000 -i .vagrant/machines/gluu-cluster-manager/lxc/private_key vagrant@gluu-cluster-manager
```

Access to the Gluu Cluster Manager:

http://localhost:6080/

Sample projects
---------------
You can find a full example of a playbook here:

https://github.com/GuillaumeSmaha/ansible-gluu-playbook

