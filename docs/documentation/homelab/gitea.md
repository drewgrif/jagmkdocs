---
tags:
    - software
    - homelab
    - gitea
    - documentation
---
# 🌐 Gitea

## ☁️  Cloudflare Setup

1. **✅ Verify Cloudflare DNS**  
   Ensure your domain is pointed to Cloudflare's DNS.  
   ![Cloudflare DNS Setup](https://github.com/user-attachments/assets/25d76502-6cfe-4c63-8c04-1a287376c9f8)

2. **🔒 Access Cloudflare Zero Trust**  
   Go to [Cloudflare Zero Trust](https://one.dash.cloudflare.com).

3. **🔍 Navigate to Network > Tunnels.**

4. **🛠️ Create a Tunnel**  
   Select the Debian 64-bit architecture.

5. **💻 Install Cloudflared on Ubuntu 24.04**  
   Copy the Cloudflared connector and install it on your server.

6. **🏷️ Configure the Public Hostname**  
   ![Public Hostname Configuration](https://github.com/user-attachments/assets/a1ce0a62-b72d-4d92-99b7-fb2f38f6cda7)

---

## 🖥️ Step 1: Update Your System

Start by updating your system packages:

```bash
sudo apt-get update
sudo apt-get upgrade
```

## 🗄️ Step 2: Install and Configure MariaDB

### 📦 Install MariaDB

Run the following commands to install MariaDB:

```bash
sudo apt install -y mariadb-server
sudo mysql_secure_installation
```

Follow the prompts:

- **Current root password:** Press `Enter`
- **Switch to unix_socket authentication:** `n`
- **Change root password:** Press `Enter` or type `Y` and set a new password.
- **Remove anonymous users:** Press `Enter` or type `Y`
- **Disallow root login remotely:** Press `Enter` or type `Y`
- **Remove test database:** Press `Enter` or type `Y`
- **Reload privilege tables:** Press `Enter` or type `Y`

### 🗃️ Create the Gitea Database

Open the MariaDB prompt:

```bash
sudo mariadb
```

Create the Gitea database and user:

```sql
CREATE DATABASE gitea;
GRANT ALL PRIVILEGES ON gitea.* TO 'gitea'@'localhost' IDENTIFIED BY 'Strong-Password';
FLUSH PRIVILEGES;
EXIT;
```

## 🚀 Step 3: Install Gitea

### 👤 Create a User for Gitea

Create a system user named `git`:

```bash
sudo adduser --system --shell /bin/bash --group --disabled-password --home /home/git git
```

### 📥 Download and Move Gitea

Download Gitea and move it to the `/usr/bin` directory:

```bash
wget https://dl.gitea.com/gitea/1.22.3/gitea-1.22.3-linux-amd64
sudo mv gitea-1.22.3-linux-amd64 /usr/bin/gitea
```

### 🔒 Set Permissions

Adjust the permissions for the Gitea binary:

```bash
sudo chmod 755 /usr/bin/gitea
```

### 📂 Create Required Directories

Set up the necessary directories for Gitea:

```bash
sudo mkdir -p /etc/gitea /var/lib/gitea/{custom,data,indexers,public,log}
sudo chown git:git /etc/gitea /var/lib/gitea/{custom,data,indexers,public,log}
sudo chmod 750 /var/lib/gitea/{data,indexers,log}
sudo chmod 770 /etc/gitea
```

### ⚙️ Create the Gitea Service File

Create a new service file for Gitea:

```bash
sudo nano /etc/systemd/system/gitea.service
```

Add the following content:

```ini
[Unit]
Description=Gitea
After=syslog.target
After=network.target

[Service]
RestartSec=3s
Type=simple
User=git
Group=git
WorkingDirectory=/var/lib/gitea/
ExecStart=/usr/bin/gitea web --config /etc/gitea/app.ini
Restart=always
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea

[Install]
WantedBy=multi-user.target
```

Save and exit the file (Ctrl + O, Enter, Ctrl + X).

### 🔄 Start the Gitea Service

Reload the systemd daemon and start the Gitea service:

```bash
sudo systemctl daemon-reload
sudo systemctl start gitea
```

Enable Gitea to start on boot:

```bash
sudo systemctl enable gitea
```

## 🌍 Step 4: Access Gitea Web Interface

Open your web browser and go to `http://<IP-address-of-your-server>:3000`. You will see Gitea's initial configuration screen.

To use Gitea with a domain, set up a reverse proxy using your preferred web server (Nginx, Apache, etc.). This will allow access to Gitea without specifying a port.

During the initial setup, use the database credentials created earlier and configure the general settings. Click ‘Install Gitea’ to complete the setup, and you will be redirected to the Gitea homepage where you can log in.

## 📝 Step 5: Modify the INI File

If you use Cloudflare to create a (sub)domain for your Gitea instance, it’s essential to modify the INI file to restrict user sign-ups.

Locate the file at `/etc/gitea/app.ini` and make the necessary adjustments.

Restart the Gitea service:

```bash
sudo systemctl restart gitea
```

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="justaguylinux" data-description="Support me on Buy me a coffee!" data-message="" data-color="#FF5F5F" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
