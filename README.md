# Ansible Role: WordPress Automation

This project automates the installation and configuration of a **LAMP Stack** (Linux, Apache, MySQL/MariaDB, PHP) and **WordPress** on Ubuntu/Debian servers using **Ansible**.

## 🚀 Features

This role performs the following tasks:
- **System Update:** Updates apt cache and installs necessary dependencies.
- **LAMP Stack:** Installs Apache2, MariaDB, and PHP (with required extensions).
- **Database Setup:** Creates a dedicated database and user for WordPress.
- **WordPress Installation:** Downloads the latest WordPress version and configures `wp-config.php`.
- **Virtual Host:** Configures Apache VirtualHost and enables the site.
- **Security:** Uses `ansible-vault` to protect sensitive data (database passwords).

## 📂 Project Structure

```text
.
├── ansible.cfg         # Ansible configuration (privilege escalation, inventory path)
├── inventory.ini       # Server inventory file
├── site.yml            # Main Playbook
├── roles/
│   └── wordpress/
│       ├── tasks/      # Main installation tasks
│       ├── handlers/   # Service restart handlers
│       ├── templates/  # Jinja2 templates (vhost, wp-config)
│       ├── defaults/   # Default variables
│       └── meta/       # Role metadata
└── host_vars/          # Encrypted host-specific variables