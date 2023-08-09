# Ansible Playbooks

This repository configures the base setup for servers that @wheresalice manages.

* services.wheresalice.info

## Onboarding

Note that I probably won't approve this unless I already know you

### services.wheresalice.info

1. Raise an issue to request access, which will include the private git repo for managing docker compose
2. Raise a PR to get yourself added to `group_vars/wheresalice.yml` to get ssh access to the servers

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
ansible all -m ping -i hosts

# converge all servers
ansible-playbook site.yml -v -i hosts

# converge services.wheresalice.info
ansible-playbook wheresalice.yml -v -i hosts

# lint the ansible code
docker run --rm -ti -v `pwd`:/src/playbook -w /src/playbook ghcr.io/ansible/creator-ee:latest ansible-lint

```

## Configuration files

The `hosts` file defines all hosts and groups which they belong to. Note that a single host can be member of multiple groups. Define groups for each rack, for each network, or for each environment (e.g. production vs. test).

### Playbooks & Roles

The group playbooks (e.g. `wheresalice.yml`) simply associate hosts with roles

#### wheresalice.yml

| role               | description                                                      |
|--------------------|------------------------------------------------------------------|
| common             | create and manage users and common base setup across all servers |
| geerlingguy.docker | install Docker                                                   |
| oefenweb.fail2ban  | configure fail2ban                                               |
| ssh_hardening      | harden ssh configuration from devsec.hardening                   |
| os_hardening       | harden os configuration from devsec.hardening                    |

