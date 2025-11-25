

-----

````markdown
# Frappe 14 Installation Guide (Ubuntu 22.04 LTS)

A step-by-step guide to installing Frappe Framework (Version 14) and ERPNext on a clean Ubuntu 22.04 LTS server.

## Prerequisites
* **OS:** Ubuntu 22.04 LTS
* **User:** A non-root user with sudo privileges (Frappe should not be installed as root).

---

## Installation Steps

### 1. Update and Upgrade Packages
First, ensure your system packages are up to date.

```bash
sudo apt-get update -y
sudo apt-get upgrade -y
````

### 2\. Install Required Dependencies

Install Git, Python dev tools, and pip.

```bash
sudo apt-get install git -y
sudo apt-get install python3-dev python3-setuptools python3-pip -y
sudo apt install python3.12-venv -y
```

### 3\. Install MariaDB

Install the MariaDB database server and required libraries.

```bash
sudo apt-get install software-properties-common -y
sudo apt install mariadb-server -y
```

#### Secure the MySQL Installation

Run the security script:

```bash
sudo mysql_secure_installation
```

**Follow the prompts as shown below:**

  * Enter current password for root: **(Press Enter if none, or type your SSH root password)**
  * Switch to unix\_socket authentication: **Y**
  * Change root password: **Y**
  * Remove anonymous users: **Y**
  * Disallow root login remotely: **N**
  * Remove test database: **Y**
  * Reload privilege tables: **Y**

#### Configure MariaDB

Open the configuration file:

```bash
sudo nano /etc/mysql/my.cnf
```

Add the following configuration at the end of the file:

```ini
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```

Save and exit (Ctrl+O, Enter, Ctrl+X), then restart the service:

```bash
sudo service mysql restart
```

### 4\. Install Redis Server

```bash
sudo apt-get install redis-server -y
```

### 5\. Install CURL, Node (via NVM), NPM, and Yarn

Frappe 14 requires Node.js (Version 16 is recommended).

```bash
# Install CURL
sudo apt install curl -y

# Install NVM (Node Version Manager)
curl [https://raw.githubusercontent.com/creationix/nvm/master/install.sh](https://raw.githubusercontent.com/creationix/nvm/master/install.sh) | bash

# Load NVM
source ~/.profile

# Install Node 16
nvm install 16

# Install NPM and Yarn
sudo apt-get install npm -y
sudo npm install -g yarn -y
```

### 6\. Install wkhtmltopdf

Required for generating PDF documents in ERPNext.

```bash
sudo apt-get install xvfb libfontconfig wkhtmltopdf -y
```

### 7\. Install Frappe Bench

Install the CLI tool used to manage Frappe deployments.

```bash
sudo -H pip3 install frappe-bench
```

### 8\. Initialize Bench and Install Apps

**Initialize the Bench:**
Replace `[folder_name]` with your desired directory name.

```bash
bench init frappe-bench --frappe-branch version-14 [folder_name]
cd frappe-bench
```

**Adjust Permissions:**
Replace `[frappe-user]` with your current system username.

```bash
chmod -R o+rx /home/[frappe-user]
```

**Create a New Site:**
Replace `[site-name]` with your domain or site name (e.g., `site1.local`).

```bash
bench new-site [site-name]
```

**Set the site as default:**

```bash
bench use [site-name]
```

### 9\. Install ERPNext and HRMS

Download and install the specific apps for Version 14.

```bash
# Get the apps
bench get-app payments
bench get-app --branch version-14 erpnext
bench get-app --branch version-14 hrms

# Install apps on your site
bench --site [site-name] install-app erpnext
bench --site [site-name] install-app hrms
```

### 10\. Start the Server

Start the bench to run the application in development mode.

```bash
bench start
```

You can now access your installation at `http://localhost:8000` (or your server IP).
http://localhost:8000
```


