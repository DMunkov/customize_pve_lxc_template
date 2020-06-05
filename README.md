Ansible role to build a new customized CentOS 8 LXC template on Proxmox.
=========

**The problem**

Default CentOS LXC template does not have openssh-server installed, which makes managing these machine difficult.
The solution was found to deploy new LXC from a reference template, modify it, back it up using built-in `vzdump` tool and upload to your storage. More on the manual steps can be found here:
https://www.nathancurry.com/blog/15-customize-lxc-container-template-on-proxmox/

**The solution**

This playbook offers an easy way of creating customized templates. Modify the vars, run the role on your Proxmox host and start using your updated LXC template (`template_name`)

Requirements
------------

Standard `proxmox` Ansible module requirements: https://docs.ansible.com/ansible/latest/modules/proxmox_module.html

As of Ansible version 2.9:
* proxmoxer
* python >= 2.7
* requests

* openssl - for encrypting passwords

Role Variables
--------------

Check example variables file below for comprehensive list of settable variables.

Dependencies
------------

No dependencies

Example variables file
----------------
`tpl_vars.yml`

``` yml
# These variables can be either passed through role vars or hosts/group_vars/host_vars files
pve_node:               "pve01"     # Your Proxmox node name (eg pve01)
pve_api_user:           "root@pam"  # Proxmox API user (eg root@pam)
pve_api_password:       "password"  # API user password (usually same as root)
pve_api_host:           "pve01"     # Same as Proxmox node name (unless you are into clusters)

# Reference template must be downloaded prior to running this role
templare_ref:           "centos-8-default_20191016_amd64.tar.xz"
# Proxmox storage name for storing container templates
template_ref_storage:   "iso"
template_ref_uri:       "{{ template_ref_storage }}:vztmpl/{{ templare_ref }}"

# New template name, should end with .tar.gz
# Passwords can be stored with ansible-vault (more here: https://docs.ansible.com/ansible/latest/user_guide/vault.html)
template_name:          "centos-8-ssh-20200604_amd64.tar.gz"
template_storage:       "iso"       # Proxmox storage name for storing container templates
template_id:            "5215"      # Any unused Proxmox vmid
template_root_password: "password"  # New password for root
template_hostname:      "centos8"   # Template name
template_new_user:      "ansible"   # Will create a new administrator account
template_new_user_pass: "password"  # Password for this new user account

# Temporary directory to store vzdump (backup) file for a new template. Local to your Proxmox instance.
template_temp_dir:      "~/{{ template_id }}-{{ template_hostname }}"
```

Example Playbook
----------------

``` yml
- hosts: proxmox01
  vars_files:
    - tpl_vars.yml
  roles:
    - dmunkov.customize_pve_lxc_template
```

License
-------

[MIT](LICENSE)

Author Information
------------------

[DMunkov GitHub](https://github.com/DMunkov)
