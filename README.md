# Ansible Playbooks

This repository configures the base setup for servers that @wheresalice manages

## Using this repository

Install a recent version of Ansible first

Then clone the repository and install dependencies:

```
git clone https://github.com/wheresalice/docker-ansible.git docker-ansible/
ansible-galaxy install -r requirements.yml
```

## Using Ansible


```
# Check your setup
ansible all -m ping -u root

# converge all servers
ansible-playbook site.yml -v -i hosts

```

## Configuration files

The `hosts` file defines all hosts and groups which they belong to. Note that a single host can be member of multiple groups. Define groups for each rack, for each network, or for each environment (e.g. production vs. test).

### Roles

The group playbooks (e.g. `anygroup.yml`) simply associate hosts with roles. Most local tasks are configured in the `common` role.

| role               | description                                                      |
|--------------------|------------------------------------------------------------------|
| common             | create and manage users and common base setup across all servers |
| geerlingguy.docker | install Docker                                                   |
| oefenweb.fail2ban  | configure fail2ban                                               |
| ssh_hardening      | harden ssh configuration from devsec.hardening                   |
| os_hardening       | harden os configuration from devsec.hardening                    |
