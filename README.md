# Lab 7: Securing Sensitive Data with Ansible Vault

## 📌 Overview
This lab demonstrates how to:
- Install and configure **MariaDB** on an **Amazon Linux 2023** EC2 instance.
- Create a **secure database setup** using **Ansible Vault** for sensitive data.
- Validate the installation by connecting to the database and listing available databases.

---

## 🗂 Project Structure
```
ansible-lab7/
├── inventory.ini # Ansible inventory with EC2 public IP & SSH key
├── playbook.yaml # Main Ansible playbook
├── vars/
│ └── vault.yml # Encrypted variables file (Ansible Vault)
├── templates/
│ └── create_db.sql.j2 # SQL template for creating DB and user
```
---

## 🔐 Ansible Vault Variables
The `vars/vault.yml` file contains sensitive variables such as:
```
mysql_root_password: root1234
db_user: ivolve_user
db_user_password: securepass123
```
Create Vault file:
```
ansible-vault create vars/vault.yml
```
Edit Vault file:
```
ansible-vault edit vars/vault.yml
```
📜 Playbook Details
Tasks Performed
- Install MariaDB 10.11 on Amazon Linux 2023.

- Start and enable the MariaDB service.

- Set the MariaDB root password.

- Create a new database named iVolve.

- Create a new user with full privileges on iVolve DB.

- Validate the database connection using the created user.

🔧 How to Run
1️⃣ Update `inventory.ini` with your EC2 details
```
[db]
54.147.167.131 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/path_to_private_key
```
2️⃣ Run the playbook
```
ansible-playbook -i inventory.ini playbook.yaml --ask-vault-pass
```
Enter your Ansible Vault password when prompted.

✅ Validation
SSH into EC2
```
ssh -i ~/.ssh/path_to_private_key ec2-user@ec2_public_ip
```
Log in to MariaDB
```
mysql -u ivolve_user -p
```
List databases
```
SHOW DATABASES;
```
Expected result:
```
+--------------------+
| Database           |
+--------------------+
| iVolve             |
| information_schema |
+--------------------+
```
