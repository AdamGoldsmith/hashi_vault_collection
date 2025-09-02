# ansible-role-vault-upgrade

Upgrades Hashicorp's Vault by
* Downloading & unzipping Vault from Hashicorp's releases site into /usr/bin

Currently tested on these Operating Systems
* Debian/Stretch64

## Requirements

* Ansible 2.5 or higher
* __Centos__ : Enable EPEL repository

  EPEL is not enabled in this role, try Jeff Geerling's [EPEL role](<https://galaxy.ansible.com/geerlingguy/repo-epel/>)

## Role Variables

`defaults/main.yml`
```yaml
vault_src: "https://releases.hashicorp.com/vault/0.10.3/vault_1.9.3_linux_amd64.zip"	# Version of vault to download from Hashicorp's website
vault_chksum: "sha256:16059f245fb4df2800fe6ba320ea66aba9c2615348e37bcfd42754591a528639"	# Vault download file checksum
vault_bin_path: "/usr/bin"								# Path to install vault binary
```

## Dependencies

Requires elevated root privileges

## Example Playbook

```yaml
---

- name: Upgrade Hashicorp Vault
  hosts: vault
  become: yes
  gather_facts: yes

  roles:
    - ansible-role-vault-stop
    - ansible-role-vault-upgrade
    - ansible-role-vault-start
    - ansible-role-vault-unseal
```

## Example execution

Repeat and update the `--limit` target host for each node in the Vault cluster
```sh
ansible-playbook vault_upgrade.yml --limit vault-01
```
## License

MIT License

## Author Information

Adam Goldsmith
