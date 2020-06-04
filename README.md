Ansible role to build a new customized CentOS 8 LXC template on Proxmox.
=========

**The problem**

Default CentOS LXC template does not have openssh-server installed, which makes managing these machine difficult.
The solution was found to deploy new LXC from a reference template, modify it, back it up using built-in `vzdump` tool and upload to your storage. More on the manual steps can be found here:
https://www.nathancurry.com/blog/15-customize-lxc-container-template-on-proxmox/

**The solution**

This playbook offers an easy way of creating customized templates. Modify the [vars](vars\main.yml), run the role on your Proxmox host and start using your updated LXC template (`template_name`)

Requirements
------------

Standard `proxmox` Ansible module requirements: https://docs.ansible.com/ansible/latest/modules/proxmox_module.html

As of Ansible version 2.9:
* proxmoxer
* python >= 2.7
* requests

Role Variables
--------------

Check [vars\main.yml](vars\main.yml) for comprehensive list of settable variables.

Dependencies
------------

No dependencies

Example Playbook
----------------

    - hosts: proxmox01
      vars:
        # Unless specified in hosts/group_vars/host_vars, Proxmox credentials
        ansible_user: root
        ansible_password: password
        api_password: "{{ ansible_password }}"
      roles:
        - dmunkov.customize_pve_lxc_template
          

License
-------

[MIT](LICENSE)

Author Information
------------------

[DMunkov GitHub](https://github.com/DMunkov)
