# 🚀 Ansible EC2 Setup: MySQL + PostgreSQL + Nginx (with Vault)

This project automates the setup of a web and database stack on a manually launched **EC2 instance** using **Ansible**. It includes:

- ✅ MySQL  
- ✅ PostgreSQL  
- ✅ Nginx  
- 🔐 Secure password management using **Ansible Vault**

---

## 📁 Project Structure

ansible-intern-task/
├── inventory/
│   └── hosts                  # Inventory file with EC2 IP
├── group_vars/
│   └── all.yml                # Encrypted variables using Ansible Vault
├── roles/
│   ├── mysql/
│   │   └── tasks/
│   │       └── main.yml       # MySQL installation tasks
│   ├── postgresql/
│   │   └── tasks/
│   │       └── main.yml       # PostgreSQL installation tasks
│   └── nginx/
│       └── tasks/
│           └── main.yml       # Nginx installation tasks
├── ec2_key.pem                # Your private key for EC2 access
├── site.yml                   # Master playbook to run all roles

---

## ✅ Step-by-Step Guide

### ✅ Step 1: Launch EC2 Instance (Manually)

1. Go to AWS Console → EC2 → Launch Instance  
2. Configuration:
   - **Name**: `target-server`
   - **AMI**: Ubuntu 22.04 LTS
   - **Instance Type**: `t2.micro`
   - **Key Pair**: Create/download `.pem` (save as `ec2_key.pem`)
3. Under **Security Group**, allow:
   - SSH (22)
   - HTTP (80)
   - MySQL (3306)
   - PostgreSQL (5432)
4. Launch and note the **Public IPv4**

---

### ✅ Step 2: SSH into EC2

```bash
chmod 400 ec2_key.pem
ssh -i ec2_key.pem ubuntu@<your-ec2-public-ip>
```

---

### ✅ Step 3: Install Ansible (on your local machine)

```bash
sudo apt update
sudo apt install ansible -y
```

---

### ✅ Step 4: Configure Inventory

Create `inventory/hosts`:

```
[web]
ubuntu@<your-ec2-ip> ansible_ssh_private_key_file=ec2_key.pem ansible_user=ubuntu
```

---

### ✅ Step 5: Create Vault File for Passwords

```bash
ansible-vault create group_vars/all.yml
```

Add the following inside:

```yaml
mysql_root_password: "YourMySQLPassword"
postgres_password: "YourPostgresPassword"
```

---

### ✅ Step 6: Create `site.yml` Playbook

```yaml
- name: Setup Web and Database Stack
  hosts: web
  become: true
  roles:
    - mysql
    - postgresql
    - nginx
```

---

### ✅ Step 7: Run the Playbook

```bash
ansible-playbook -i inventory/hosts site.yml --ask-vault-pass
```

---

## ✅ What This Does

- SSH into the EC2 instance  
- Install MySQL, PostgreSQL, and Nginx  
- Secure credentials using Ansible Vault  
- Enable and start all services automatically  

---

