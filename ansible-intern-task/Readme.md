# 🚀 Ansible EC2 Setup: MySQL + PostgreSQL + Nginx (with Vault)

This project automates the setup of a web and database stack on a manually launched EC2 instance using **Ansible**, including:

- ✅ MySQL
- ✅ PostgreSQL
- ✅ Nginx  
- 🔐 Secure password management using **Ansible Vault**

---

## 📁 Project Structure

ansible-intern-task/
├── inventory/
│ └── hosts # Inventory file with EC2 IP
├── group_vars/
│ └── all.yml # Encrypted variables using Ansible Vault
├── roles/
│ ├── mysql/
│ │ └── tasks/
│ │ └── main.yml # MySQL installation tasks
│ ├── postgresql/
│ │ └── tasks/
│ │ └── main.yml # PostgreSQL installation tasks
│ └── nginx/
│ └── tasks/
│ └── main.yml # Nginx installation tasks
├── ec2_key.pem # Your private key for EC2 access
├── site.yml # Master playbook to run all roles



---

## ✅ Step 1: Launch EC2 Manually (Ubuntu 22.04)

1. Go to AWS Console → EC2 → Launch Instance
2. Choose:
   - **Name**: `target-server`
   - **AMI**: Ubuntu 22.04 LTS
   - **Instance type**: `t2.micro`
   - **Key Pair**: Create/download `.pem` file
3. Under **Security Group**, allow:
   - SSH (22)
   - HTTP (80)
   - MySQL (3306)
   - PostgreSQL (5432)
4. Launch the instance and note the **Public IPv4**

---

## ✅ Step 2: SSH into EC2 (from your machine)

```bash
chmod 400 ec2_key.pem
ssh -i ec2_key.pem ubuntu@<your-ec2-public-ip>


Step 3: Install Ansible (on your local machine)

sudo apt update
sudo apt install ansible -y

✅ Step 4: Prepare Your Ansible Project

[web]
ubuntu@<your-ec2-ip> ansible_ssh_private_key_file=ec2_key.pem ansible_user=ubuntu

✅ Step 5: Create Vault File for Passwords

ansible-vault create group_vars/all.yml
then addd mysql_root_password: "YourMySQLPassword"
postgres_password: "YourPostgresPassword"

✅ Step 6: Create Your site.yml Playbook

✅ Step 7: Run the Playbook

ansible-playbook -i inventory/hosts site.yml --ask-vault-pass

This will:

SSH into the EC2 instance

Install MySQL, PostgreSQL, and Nginx

Set secure passwords from Vault

Ensure all services are enabled and running
