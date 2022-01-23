# Ansible cheatsheet

- [Ansible cheatsheet](#ansible-cheatsheet)
  - [prerequisites](#prerequisites)
  - [Ansible Setup](#ansible-setup)

## prerequisites

1. Install OS
2. Maak user aan voor Ansible
3. Public key naar server kopiëren (manueel of met `ssh-copy-id`)

## Ansible Setup

In de folder waarin je werkt kan je een `ansible.cfg` bestand maken om de standaard configuratie over te overschrijven.

```ini
# ansible.cfg
[defaults]
# verwijzing naar de inventoryfile
inventory = hosts
# ssh prompt (meestal eerste keer voor een connect)
host_key_checking = True

# username van de remote user
remote_user = vagrant
# ssh key
#private_key_file = ~/.ssh/ssh-key
```
