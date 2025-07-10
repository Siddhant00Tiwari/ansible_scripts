# Ansible EC2 Setup - MySQL, Redis, Docker Swarm

Automated installation of MySQL, Redis, and Docker Swarm on Amazon Linux 2023 EC2 instance.

## Quick Start

1. **Setup files:**
   ```bash
   mkdir ansible-ec2-setup && cd ansible-ec2-setup
   mkdir templates
   # Copy site.yml and create templates/my.cnf.j2
   ```

2. **Fix SSH key permissions:**
   ```bash
   cp your-key.pem ~/ec2.pem
   chmod 400 ~/ec2.pem
   ```

3. **Create inventory.yml:**
   ```yaml
   all:
     children:
       ec2_instance:
         hosts:
           single-node:
             ansible_host: YOUR_EC2_IP
             ansible_user: ec2-user
             ansible_ssh_private_key_file: ~/ec2.pem
   ```

4. **Run playbook:**
   ```bash
   ansible-playbook -i inventory.yml site.yml
   ```

## What Gets Installed

- **MySQL 8.0** - Root password: `StrongPass123!`
- **Redis 6** - Accessible on port 6379
- **Docker + Swarm** - Single-node cluster initialized

## Post-Installation

```bash
# Test MySQL
mysql -uroot -p'StrongPass123!' -e "SELECT VERSION();"

# Test Redis
redis-cli ping

# Test Docker Swarm
sudo docker node ls
```

## Requirements

- Amazon Linux 2023 EC2 instance
- SSH access with key pair
- Security group allowing ports: 22, 3306, 6379, 2377, 7946, 4789

## Files

- `site.yml` - Main Ansible playbook
- `inventory.yml` - EC2 instance configuration
- `templates/my.cnf.j2` - MySQL configuration template