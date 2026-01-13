# Playbooks

This directory contains Ansible playbooks for deploying and managing the multi-tier web application infrastructure.

## Available Playbooks

### site.yml
Main playbook that orchestrates the complete infrastructure deployment.

**Usage:**
```bash
ansible-playbook -i inventories/dev/hosts playbooks/site.yml
```

**What it does:**
- Applies common configuration to all servers
- Deploys Nginx web servers
- Deploys HAProxy load balancers
- Displays deployment summary with access URLs

---

### common.yml
Applies base system configuration to all hosts.

**Usage:**
```bash
ansible-playbook -i inventories/dev/hosts playbooks/common.yml
```

**What it does:**
- Updates packages and installs common tools
- Configures firewall, SSH, and NTP
- Applies security hardening

---

### webservers.yml
Deploys and configures Nginx web servers.

**Usage:**
```bash
ansible-playbook -i inventories/dev/hosts playbooks/webservers.yml
```

**What it does:**
- Applies common configuration
- Installs and configures Nginx
- Generates SSL certificates
- Deploys web application
- Verifies deployment with health checks

---

### loadbalancers.yml
Deploys and configures HAProxy load balancers.

**Usage:**
```bash
ansible-playbook -i inventories/dev/hosts playbooks/loadbalancers.yml
```

**What it does:**
- Applies common configuration
- Installs and configures HAProxy
- Dynamically configures backend servers from inventory
- Sets up health checks and stats page

---

### validate.yml
Tests and validates the deployed infrastructure.

**Usage:**
```bash
ansible-playbook -i inventories/dev/hosts playbooks/validate.yml
```

**What it does:**
- Checks firewall and service status
- Tests HTTP responses from web servers
- Validates health check endpoints
- Tests load balancer distribution
- Verifies HAProxy stats page accessibility

---

## Quick Start

### Full deployment:
```bash
ansible-playbook -i inventories/dev/hosts playbooks/site.yml
```

### Dry run (test without changes):
```bash
ansible-playbook -i inventories/dev/hosts playbooks/site.yml --check
```

### Deploy with specific tags:
```bash
# Only installation tasks
ansible-playbook -i inventories/dev/hosts playbooks/site.yml --tags install

# Only configuration tasks
ansible-playbook -i inventories/dev/hosts playbooks/site.yml --tags configure
```

### Validate deployment:
```bash
ansible-playbook -i inventories/dev/hosts playbooks/validate.yml
```

---

## Multi-Environment Support

### Development:
```bash
ansible-playbook -i inventories/dev/hosts playbooks/site.yml
```

### Staging:
```bash
ansible-playbook -i inventories/staging/hosts playbooks/site.yml
```

### Production:
```bash
ansible-playbook -i inventories/production/hosts playbooks/site.yml
```

---

## Tags

All playbooks support tags for granular control:

- `common` - Common configuration tasks
- `install` - Installation tasks
- `configure` - Configuration tasks
- `ssl` - SSL certificate tasks
- `deploy` - Application deployment tasks
- `firewall` - Firewall configuration tasks
- `webserver` - Web server specific tasks
- `loadbalancer` - Load balancer specific tasks

---

## Troubleshooting

### Check syntax:
```bash
ansible-playbook playbooks/site.yml --syntax-check
```

### List tasks:
```bash
ansible-playbook -i inventories/dev/hosts playbooks/site.yml --list-tasks
```

### Verbose output:
```bash
ansible-playbook -i inventories/dev/hosts playbooks/site.yml -vv
```
