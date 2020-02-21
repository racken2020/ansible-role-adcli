# Ansible role for joining AD with adcli
URL: https://xpk.headdesk.me/git/xpk/role.adcli.git

Note that ad_netbios_name will default to inventory hostname if not supplied. That said, hostname must be specified in the inventory file.

Writes adcli output to /var/log/adcli.log

## Set required variables in group_vars/all.yml
ad_domain: some-domain.tld
ad_dc1: 1.2.3.4
ad_dc2: 2.3.4.5
ad_joinusr: adjoin
ad_joinpw: xxx

## Optional variable:
- ad_sudoers_group
- ad_netbios_name (note this is a host variable, useful when hostname is longer than the netbios limit of 15 characters)

## Sample playbook utilizing this role
Here variables are set in the inventory. One may prefer setting the in group_vars/ so they can be encrypted

```
- name: Join stupid AD
  hosts: a-hostname-with-more-than-15-characters
  become: yes
  roles:
    - role: adcli
```

## Sample inventory
```
a-hostname-with-more-than-15-characters ansible_host=192.168.1.101 ad_netbios_name=shorterMe
```

## Pre-checks
Check that the target machines have access to AD controller on these ports: 53, 88, 389, 445. e.g.
```
nmap -p53,88,389,445 <ad controller ip>
```

Do a lookup for the SRV records
```
host -tsrv _ldap._tcp.dc._msdcs.DOMAIN <DC IP>
```

## Adding this as a git submodule to your ansible home
```
git submodule add https://xpk.headdesk.me/git/xpk/role.adcli.git roles/adcli
git commit -S -m 'SUB: adcli submodule'
git push
```
