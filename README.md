*******
linux_upgrade_sles 
*******

Linux_upgrade_sles is an simple Ansible role for automate upgrade SP (Service Pack) process in SLES-based distributions.
It handles SLES-based distributions more specific with versions 12 and 15,
its porpuse is to automate upgrade activity of SP to ensure all the servers in this kind of distribution are in compliance status.
This activity is for a minor upgrade. 

Design Principles
=================

*  To help System Owners to upgrade their SLES-based servers to the latest Service Pack
*  Automate upgrade process of the Service Pack on SLES-based distributions. 
*  Apply the activity in many SLES servers at the same time
*  To Avoid human-prone errors during the activity
*  Validate if the managed host or managed hosts are SLES-based distributions, if not the proccess will be failed on it or them.
*  Validate if the managed host or managed hosts have an active subscription, if not the proccess will be failed on it or them.
*  Validate if the managed host or managed hosts are SLES version 12 or 15, if not the proccess will be failed on it or them.
*  Validate if the managed host or managed hosts are in the latest version of SP, if not the proccess will be applied on it or them.
*  The upgrade tasks will be applied depend if the SLES version is 12 or 15.
*  Is very important to have in mind that if some validation is not acomplishied in any server the Ansible task will continue with the rest of the servers

Requirements
===========

For use this ansible role you need install ansible version 2.9 or +

Use Ansible linux_upgrade_sles
===========

First you need to modify the ansible.cfg file which is in the root directory of the project,
in the defaults section inside the file you need to put your user that you use to connect to servers,
is very important that the user has sudo access to apply all the activities.
example of ansible.cfg

[defaults]
remote_user = $yourUser
inventory = inventory
deprecation_warnings = False
ask_pass = true
remote_tmp = /tmp/.ansible/tmp
callback_whitelist = profile_tasks

[privilege_escalation]
become = true
become_user = root
become_method = sudo
become_ask_pass = true

You need to modify $yourUser with your own user that you use for connecting to the servers

For use this Ansible Role you need to run the playbook infra_upgrade_sles.yml but first you need to modify the inventory.
You need to filfull the inventory file with the server of group of servers you want to applied the Ansible role.
This is an example of the content in inventory file:

[suse_servers]
suse12 ansible_host=9.13.24.12
suse15 ansible_host=9.65.12.86


In the infra_upgrade_sles.yml file the only you need to modify is in which hosts or group of hosts you want apply it.
This is an example for using the yaml file:

- name: Ensure Suse Servers version 12 are upgraded to latest SP version
  hosts: suse_servers
  roles:
    - role: linux_upgrade_sles
      when: ansible_distribution == 'SLES'
  post_tasks:
    - debug:
        msg: "The {{ ansible_fqdn }} server is not a Suse Server"
      when: ansible_distribution != 'SLES'


EXAMPLES of how to use linux_upgrade_sles
===========

#If you want to apply to all servers that you put in the hosts clause on the infra_upgrade_sles.yml file
$ ansible-playbook infra_upgrade_sles.yml 

#If you want to apply in a specific server on the group of servers that you put in the hosts clause on the infra_upgrade_sles.yml file
$ ansible-playbook infra_upgrade_sles.yml --limit suse12

   
Authors
=======

linux_upgrade_sles was created by `Ivan Herrera Perez email: ivan.herrera@ibm.com`

Licence
=======

[GPL](https://choosealicense.com/licenses/gpl-3.0/)


 
