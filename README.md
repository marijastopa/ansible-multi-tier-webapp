# Ansible Multi-Tier Web Application Infrastructure

## Project Overview

This project demonstrates a production-ready multi-tier web application infrastructure deployed using Ansible. It includes automated configuration management, load balancing, security hardening, and comprehensive testing procedures.

**Infrastructure:** HAProxy Load Balancer + Nginx Web Servers

### Components:
- **Load Balancer (lb-dev-01):** HAProxy with health checks and stats dashboard
- **Web Server 1 (web-dev-01):** Nginx with SSL/TLS
- **Web Server 2 (web-dev-02):** Nginx with SSL/TLS

---

## Features

- **Automated Deployment:** Complete infrastructure provisioning with Ansible
- **Load Balancing:** Round-robin distribution with session persistence
- **Health Monitoring:** Layer 7 HTTP health checks
- **Security:** UFW firewall, SSL/TLS certificates, Ansible Vault for secrets
- **High Availability:** Multiple web servers with automatic failover
- **Monitoring Dashboard:** HAProxy statistics page

---

## Quick Start

### Prerequisites

- Vagrant >= 2.3.0
- VirtualBox >= 7.0
- Ansible >= 2.14

### Installation

1. **Clone the repository:**
```bash
   git clone <repository-url>
   cd ansible-webapp
```

2. **Install Ansible dependencies:**
```bash
   ansible-galaxy install -r requirements.yml
```

3. **Start Vagrant VMs:**
```bash
   vagrant up
```

4. **Deploy infrastructure:**
```bash
   ansible-playbook -i inventories/dev/hosts playbooks/site.yml
```

5. **Validate deployment:**
```bash
   ansible-playbook -i inventories/dev/hosts playbooks/validate.yml
```

---

## Testing

### Access the Application

- **Load Balancer:** http://localhost:8080
- **HAProxy Stats:** http://localhost:8081/haproxy?stats
  - Username: `admin`
  - Password: `admin123`

### Test Load Balancing
```bash
for i in {1..10}; do 
  curl -I http://localhost:8080/ 2>&1 | grep "location"
done
```

Expected: Alternating requests between web-dev-01 and web-dev-02

### Verify Health Checks

Check HAProxy stats dashboard at http://localhost:8081/haproxy?stats - both servers should show as UP (green).

---

## Security

### Ansible Vault

Sensitive data is encrypted using Ansible Vault:
```bash
# View encrypted variables
ansible-vault view group_vars/all/vault.yml

# Edit encrypted variables
ansible-vault edit group_vars/all/vault.yml
```

### Firewall Rules

- UFW enabled on all servers
- Ports 80 (HTTP) and 443 (HTTPS) open
- SSH access restricted

### SSL/TLS

Self-signed certificates generated automatically during deployment.

---

## Monitoring

### HAProxy Statistics

Access: http://localhost:8081/haproxy?stats

Metrics available:
- Request rate
- Response times
- Server health status
- Session information
- Error rates

---

## Development

### Add New Web Server

1. Update `Vagrantfile` with new VM
2. Add to `inventories/dev/hosts`
3. Run: `ansible-playbook -i inventories/dev/hosts playbooks/site.yml`

### Modify Configuration

Templates located in `roles/*/templates/`  
Variables in `group_vars/all/main.yml`