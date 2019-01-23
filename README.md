# Ansible role for joining AD with adcli
URL: https://xpk.headdesk.me/git/xpk/role.adcli.git

Note that ad_netbios_name will default to inventory hostname if not supplied. That said, hostname must be specified in the inventory file.

Writes adcli output to /var/log/adcli.log

## Required variables:
- ad_domain 
- ad_dc1
- ad_dc2
- ad_joinusr
- ad_joinpw

## Optional variable:
- ad_sudoers_group
- ad_netbios_name (note this is a host variable, useful when hostname is longer than the netbios limit of 15 characters)

## Sample playbook utilizing this role
```
- name: Join stupid AD
  hosts: a-hostname-with-more-than-15-characters
  become: yes
  roles:
    - role: adcli
      vars:
        - ad_domain: foo.local
        - ad_dc1: 192.168.1.10
        - ad_dc2: 192.168.1.11
        - ad_joinusr: adjoin
        - ad_joinpw: adjoin-password
        - ad_sudoers_group: linuxadmins
```

## Sample inventory
```
a-hostname-with-more-than-15-characters ansible_host=192.168.1.101 ad_netbios_name=shorterMe
```

