# Ansible Production Server Setup

This Ansible project is designed to prepare and harden production servers with a simple and customizable structure.

## Features

This playbook handles common production server requirements:

* DNS configuration
* SSH hardening and SSH port configuration
* Removing unnecessary users
* Creating required users
* Installing basic tools (`mtr`, `dig`, `tree`, etc.)
* Updating and upgrading system packages
* Directory permission management

The repository is intentionally kept simple so you can easily modify it based on your environment needs.

---

## Project Structure

```text
.
├── ansible.cfg
├── group_vars
│   └── all.yaml
├── inventory
│   └── production
│       ├── group_vars -> /root/ansible/group_vars
│       └── hosts.ini
├── playbook
│   └── site.yaml
├── README.md
└── roles
    └── common
        ├── files
        │   └── etc
        │       └── resolv.conf
        └── tasks
            ├── apps.yaml
            ├── delete-user.yaml
            ├── dns.yaml
            ├── main.yaml
            ├── permissio_directory.yaml
            └── ssh.yaml
```

---

## Configuration

All required variables are located in:

```bash
group_vars/all.yaml
```

You can modify this file based on your server requirements.

---

## Inventory Variables

Each inventory uses the shared variables from the main `group_vars` directory.

Before running the playbook, create the symbolic link inside your inventory directory:

```bash
ln -s ../../group_vars inventory/production/group_vars
```

This allows the inventory to load variables from the shared `group_vars` directory.

---

## SSH Configuration

For SSH hardening, the configuration is managed in:

```bash
roles/common/tasks/ssh.yaml
```

The default SSH port has been changed from:

```text
22
```

to:

```text
2000
```

The SSH port value is defined in:

```bash
group_vars/all.yaml
```

Make sure your firewall rules allow the configured SSH port before applying the playbook.

---

## Run Playbook

### Check connectivity

```bash
ansible all -i inventory/production/hosts.ini -m ping
```

### Syntax check

```bash
ansible-playbook \
-i inventory/production/hosts.ini \
playbook/site.yaml \
--syntax-check
```

### Run

```bash
ansible-playbook \
-i inventory/production/hosts.ini \
playbook/site.yaml
```

---

## Notes

* Review variables before running in production.
* Test changes with `--check --diff` before applying.
* Keep an active SSH session when changing SSH configuration.
* Customize tasks and variables according to your environment.

