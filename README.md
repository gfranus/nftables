# Ansible role: nftables
---

Ansible role that configures the Linux netfilter with nftables (with logging oprion) commandline interface (basic version).

## Main actions

* Update packages cache.
* Install ACL and rsyslog packages.
* Disable firewalld service (if exists).
* Disable iptables service (if exists).
* Disable netfilter-persistent service (if exists).
* Disable ufw service (if exists).
* Reboot system (if required).
* Install nftables package.
* Configure and enable nftables service.
* Deploy base rules for nftables.
* Configure logrotate for nftables (optional).

## Requirements

#### Ansible version

Ansible >= 2.13

#### Python version

Python >= 3.9

#### Setup module
The role uses facts gathered by Ansible on the remote host. If you disable the Setup module in your playbook, the role will not work properly.

#### Root access
This role requires root access, so either configure it in your inventory files, run it in a playbook with a global `become: true` or invoke the role in your playbook like:
> site.yml:
```yaml
- hosts: servers
  become: true
  roles:
    - role: nftables
```

## Role Variables

All variables for this role are declared in the following files:
```yaml
  - default/main.yml
  - vars/*.yml
```

## Dependencies

This role does not have any dependencies.

## Example Playbook

Using the role is fairly straightforward:
> site.yml:
```yaml
- hosts: servers
  become: true
  roles:
    - role: nftables
      vars:
        - nftables_role_action: all
        - nftables_configure_logrotate: true
        - nftables_input_chain_rules:
          - 'ip saddr 0.0.0.0/0 tcp dport { 22 } ct state { new } limit rate 10/minute log prefix "[nftables] : accept : input : ssh : " counter accept comment "ssh"'
```

## Author Information

Grzegorz Franus