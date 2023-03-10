# Ansible Playbooks

This repository configures the base setup for servers that @wheresalice manages.

* shell.wheresalice.info
* services.wheresalice.info

## Onboarding

Note that I probably won't approve this unless I already know you

### services.wheresalice.info

1. Raise an issue to request access, which will include the private git repo for managing docker compose
2. Raise a PR to get yourself added to `group_vars/wheresalice.yml` to get ssh access to the servers

### shell.wheresalice.info

1. Raise a PR to get yourself added to `group_vars/shells.yml` to get ssh access to the servers
2. (optional) Raise an issue to get access to the git repo for the terraform code and terraform cloud account - you're unlikely to actually need this, it never really changes

Groups should be empty unless you need to manage the server

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

# converge shell servers
ansible-playbook shells.yml -v -i hosts

# converge services.wheresalice.info
ansible-playbook wheresalice.yml -v -i hosts

# lint the ansible code
docker run --rm -ti -v `pwd`:/src/playbook -w /src/playbook ghcr.io/ansible/creator-ee:v0.13.0 ansible-lint

# lint the ansible code in offline mode and automatically write any fixes
docker run --rm -ti -v `pwd`:/src/playbook -w /src/playbook ghcr.io/ansible/creator-ee:v0.13.0 ansible-lint --offline --write

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

#### shells.yml

| role               | description                                                      |
|--------------------|------------------------------------------------------------------|
| common             | create and manage users and common base setup across all servers |
| oefenweb.fail2ban  | configure fail2ban                                               |
| ssh_hardening      | harden ssh configuration from devsec.hardening                   |
| os_hardening       | harden os configuration from devsec.hardening                    |
