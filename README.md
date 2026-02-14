# 🚀 Automated WordPress Deployment with Ansible

This project provides a fully automated solution for deploying a secure, production-ready **WordPress** instance on **Ubuntu/Debian** servers using the **LAMP Stack** (Linux, Apache, MySQL/MariaDB, PHP).

It utilizes **Ansible Roles** for modularity and **Ansible Vault** for security, following Infrastructure as Code (IaC) best practices.



---

## 📖 Project Architecture & Design Decisions

Why did we use these specific components? Here is the breakdown of the architectural choices:

### 1. Why Ansible?
We use Ansible because it is **agentless**. unlike Puppet or Chef, we do not need to install any software on the target server effectively. It communicates over SSH, making it lightweight and secure.

### 2. Why use "Roles"?
Instead of writing one giant playbook, we used an **Ansible Role** (`roles/wordpress`).
- **Modularity:** It separates the logic into distinct parts (Tasks, Handlers, Templates).
- **Reusability:** This role can be easily reused in other playbooks or projects.
- **Maintainability:** It is easier to debug a specific task file than a 1000-line playbook.

### 3. Component Breakdown
- **`tasks/main.yml`**: The "Doers". These are the steps (install packages, create DB, download WP).
- **`handlers/main.yml`**: The "Reactors". These run *only* when notified (e.g., "Restart Apache" only happens if the config file changes). This ensures **Idempotency**.
- **`templates/*.j2`**: The "Dynamic Configs". We use Jinja2 to inject variables (like DB passwords) into configuration files (`wp-config.php`, `vhost.conf`) dynamically.
- **`defaults/main.yml`**: The "Defaults". Sets baseline variables that can be easily overridden without changing the code.

### 4. Security (Ansible Vault)
We use **Ansible Vault** to encrypt sensitive data (Database passwords, Sudo passwords).
**Why?** Committing plain-text passwords to GitHub is a major security vulnerability. Vault ensures credentials remain encrypted in the repository.

---

## 📂 Directory Structure

```text
.
├── ansible.cfg          # configuration: Defines inventory path and privilege escalation (sudo)
├── inventory.ini        # Inventory: List of target servers and connection details
├── site.yml             # Master Playbook: The entry point that calls the roles
├── roles/
│   └── wordpress/       # The core logic
│       ├── tasks/       # Steps to install LAMP & WordPress
│       ├── handlers/    # Service restart logic
│       ├── templates/   # Dynamic configuration files (.j2)
│       ├── defaults/    # Default variables
│       └── meta/        # Role metadata
└── host_vars/
    └── myserver.yml     # Encrypted variables specific to the host


Gemini said
Here is a comprehensive, professional, and educational README.md file. It is designed to impress your instructor by not only showing how to run the code but also demonstrating that you understand why the project is structured this way.

I have also included a diagram tag to visualize the process.

Instructions:
Create a file named README.md in your project folder.

Copy the code block below completely.

Save the file.

Markdown
# 🚀 Automated WordPress Deployment with Ansible

This project provides a fully automated solution for deploying a secure, production-ready **WordPress** instance on **Ubuntu/Debian** servers using the **LAMP Stack** (Linux, Apache, MySQL/MariaDB, PHP).

It utilizes **Ansible Roles** for modularity and **Ansible Vault** for security, following Infrastructure as Code (IaC) best practices.



---

## 📖 Project Architecture & Design Decisions

Why did we use these specific components? Here is the breakdown of the architectural choices:

### 1. Why Ansible?
We use Ansible because it is **agentless**. unlike Puppet or Chef, we do not need to install any software on the target server effectively. It communicates over SSH, making it lightweight and secure.

### 2. Why use "Roles"?
Instead of writing one giant playbook, we used an **Ansible Role** (`roles/wordpress`).
- **Modularity:** It separates the logic into distinct parts (Tasks, Handlers, Templates).
- **Reusability:** This role can be easily reused in other playbooks or projects.
- **Maintainability:** It is easier to debug a specific task file than a 1000-line playbook.

### 3. Component Breakdown
- **`tasks/main.yml`**: The "Doers". These are the steps (install packages, create DB, download WP).
- **`handlers/main.yml`**: The "Reactors". These run *only* when notified (e.g., "Restart Apache" only happens if the config file changes). This ensures **Idempotency**.
- **`templates/*.j2`**: The "Dynamic Configs". We use Jinja2 to inject variables (like DB passwords) into configuration files (`wp-config.php`, `vhost.conf`) dynamically.
- **`defaults/main.yml`**: The "Defaults". Sets baseline variables that can be easily overridden without changing the code.

### 4. Security (Ansible Vault)
We use **Ansible Vault** to encrypt sensitive data (Database passwords, Sudo passwords).
**Why?** Committing plain-text passwords to GitHub is a major security vulnerability. Vault ensures credentials remain encrypted in the repository.

---

## 📂 Directory Structure

```text
.
├── ansible.cfg          # configuration: Defines inventory path and privilege escalation (sudo)
├── inventory.ini        # Inventory: List of target servers and connection details
├── site.yml             # Master Playbook: The entry point that calls the roles
├── roles/
│   └── wordpress/       # The core logic
│       ├── tasks/       # Steps to install LAMP & WordPress
│       ├── handlers/    # Service restart logic
│       ├── templates/   # Dynamic configuration files (.j2)
│       ├── defaults/    # Default variables
│       └── meta/        # Role metadata
└── host_vars/
    └── myserver.yml     # Encrypted variables specific to the host
⚙️ Prerequisites
Before running this playbook, ensure you have:

Ansible installed on your control node (your laptop).

SSH Access to the target server (e.g., ssh debian@188.121.116.190 works).

Sudo Privileges for the user on the target server.

🚀 How to Run
Follow these steps to deploy WordPress:

1. Clone the Repository
Bash
git clone [https://github.com/yasinsiadat/ansible-wordpress.git](https://github.com/yasinsiadat/ansible-wordpress.git)
cd ansible-wordpress
2. Configure Inventory
Edit the inventory.ini file to match your server IP and user:

Ini, TOML
[webservers]
myserver ansible_host=YOUR_SERVER_IP ansible_user=YOUR_SSH_USER
3. Handle Security (Vault)
This project uses an encrypted file for passwords. You need the Vault Password to run it.

If you are setting this up from scratch, you can encrypt your own variables:

Bash
ansible-vault encrypt host_vars/myserver.yml
4. Execute the Playbook
Run the following command. The flag --ask-vault-pass is crucial to decrypt the passwords during execution.

Bash
ansible-playbook site.yml --ask-vault-pass
🐛 Troubleshooting
Error: Permission denied or apt lock
Cause: The user is not switching to root correctly.

Fix: Ensure ansible.cfg has become_user = root and become = True.

Error: Decryption failed
Cause: You entered the wrong Vault password.

Fix: Re-run the command and ensure you type the password used to encrypt the host_vars file.

Error: Unreachable
Cause: SSH connectivity issue.

Fix: Test connection with ansible all -m ping.

👨‍💻 Author
Yasin Siadat
Ansible Automation Project


### Summary of what I added for you:

1.  **"Why" Section:** I explicitly explained why you used Roles (separation of concerns), Handlers (efficiency/idempotency), and Vault (security). This is exactly what examiners look for.
2.  **Project Tree:** A visual representation of your files.
3.  **Troubleshooting:** I added the exact errors we fixed together (Permission denied) so anyone running it knows how to fix it.
4.  **Diagram Tag:** Included `` to visualize the controller/node relationship.
