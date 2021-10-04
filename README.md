*******
linux_upgrade_sles 
*******

Linux_upgrade_sles is an simple Ansible role for automate upgrade SP (Service Pack) in SLES-based distributions.
It handles SLES-based distributions more specific with versions 12 and 15,
its porpuse is to automate upgrade activity of SP to ensure all the servers in this kind of distribution are in compliance status.

Design Principles
=================

*  Automate upgrade process of the Service Pack on SLES-based distributions.
*  Validate if the managed host or managed hosts are SLES-based distributions, if not the proccess will be skipped on it or them.
*  Validate if the managed host or managed hosts have an active subscription, if not the proccess will be skipped on it or them.
*  Validate if the managed host or managed hosts are in version 12 or 15, if not the proccess will be skipped on it or them.
*  Validate if the managed host or managed hosts are in the latest version of SP, if not the proccess will be applied on it or them.
*  The process will be applied depend if the version is 12 or 15.

Use Ansible linux_upgrade_sles
===========

For use this Ansible Role you need to run the playbook infra_upgrade_sles.yml,
In this particular file the only you need to modify is in which hosts or group of hosts you want apply it.
This is the content of the yaml file:

- name: Ensure Suse Servers version 12 are upgraded to latest SP version
  
  hosts: $hosts or $groupOfHosts  
  roles:
    - role: linux_upgrade_sles
      when: ansible_distribution == 'SLES'
  post_tasks:
    - debug:
        msg: "The {{ ansible_fqdn }} server is not a Suse Server"
      when: ansible_distribution != 'SLES'

Also you need to filfull the inventory file with the server of group of servers you want to applied the Ansible role.
This is an example of the content in inventory file:

[suse_servers]
suse12 ansible_host=9.13.24.12
suse15 ansible_host=9.65.12.86


EXAMPLES of how to use the project

#If you want to apply to all servers that you put in the hosts clause on the infra_upgrade_sles.yml file
$ ansible-playbook infra_upgrade_sles.yml 

#If you want to apply in a specific server on the group of servers that you put in the hosts clause on the infra_upgrade_sles.yml file
$ ansible-playbook infra_upgrade_sles.yml --limit suse12

   
Authors
=======

linux_upgrade_sles was created by `Ivan Herrera Perez`

Licence
=======

[GPL](https://choosealicense.com/licenses/gpl-3.0/)


 
