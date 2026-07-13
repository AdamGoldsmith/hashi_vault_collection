# ansible-role-vault-install

Installs & configure's keepalived for Hashicorp's Vault by
* Installing keepalived package
* Creating health check script (/usr/local/bin/vault-health.sh)
* Starting & enabling keepalived systemd service

Currently tested on these Operating Systems
* Debian/Stretch64

## Requirements

* Ansible 2.5 or higher

## Role Variables

`defaults/main.yml`
```yaml
vault_keepalived_interface: "{{ ansible_default_ipv4.interface }}"                              # Interface to associate keepalived service
vault_keepalived_vip: 192.168.10.50                                                             # Keepalived Virtual IP address
vault_keepalived_prefix: 24                                                                     # CIDR prefix length of the VIP
vault_keepalived_router_id: 50                                                                  # VRRP's Virtual Router ID [VRID] (Cluster ID)
vault_keepalived_auth_pass: CHANGE_ME                                                           # VRRP's shared authentication password
vault_keepalived_priority: 100                                                                  # Cluster node's priority
vault_keepalived_advert_int: 1                                                                  # Keepalived advertisement interval
vault_keepalived_nodes: "{{ groups['vault'] }}"                                                 # Keepalived cluster nodes
vault_keepalived_config: /etc/keepalived/keepalived.conf                                        # Keepalived config file
vault_keepalived_script: /usr/local/bin/vault-health.sh                                         # Keepalived health check script for Vault service
vault_tls_disable: false                                                                        # Vault using TLS
vault_certs_dir: /etc/vault/certs                                                               # Vault certificates directory
vault_certfile: "{{ vault_certs_dir }}/{{ ansible_fqdn | regex_replace('^www\\.', '') }}.crt"   # Vault certificate file
```

## Dependencies

Requires elevated root privileges

## Example Playbook

```yaml
---

- name: Install & Configure Keepalived for Hashicorp Vault
  hosts: vault
  become: yes
  gather_facts: yes

  roles:
    - ansible-role-vault-keepalived
```

## License

MIT License

## Author Information

Adam Goldsmith
