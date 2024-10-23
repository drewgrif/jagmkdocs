---
tags:
    - software
    - homelab
    - gitea
    - documentation
---

## Setting Up Cloudflare

1. **Ensure Your Domain Uses Cloudflare DNS**
   ![2024-09-30_16-55](https://github.com/user-attachments/assets/25d76502-6cfe-4c63-8c04-1a287376c9f8)

2. **[Cloudflare Zero Trust](https://one.dash.cloudflare.com)**

3. **Navigate to Network > Tunnels**.

4. **Create the tunnel and select Debian 64 Bit architecture.**

5. **Copy the Cloudflared Connector and install on Ubuntu 24.04 server**

6. **Configure Public Hostname**  
 ![2024-09-30_17-18](https://github.com/user-attachments/assets/a1ce0a62-b72d-4d92-99b7-fb2f38f6cda7)

---

## Step 1: Update the System

Begin by updating your system packages to the latest versions. Run the following commands:

```bash
sudo apt-get update
sudo apt-get upgrade
```

## Step 2: Install and Configure the Database Server (MariaDB)

To install the MariaDB database server, use:

**Install MariaDB:**

    ```bash
    sudo apt install -y mariadb-server
    sudo mysql_secure_installation
    ```

    Follow these prompts:
    
    - Current root password: `Enter`
    
    - Switch to unix_socket authentication: `n`
    
    - Change root password: `Enter or Y` and set a new password.
    
    - Remove anonymous users: `Enter or Y`
    
    - Disallow root login remotely: `Enter or Y`
    
    - Remove test database: `Enter or Y`
    
    - Reload privilege tables: `Enter or Y`
    

**Create Gitea Database:**

    ```bash
    sudo mariadb
    ```

Next, set up the database for Gitea by creating a database and a user with all privileges:

```sql
CREATE DATABASE gitea;
GRANT ALL PRIVILEGES ON gitea.* TO 'gitea'@'localhost' IDENTIFIED BY 'Strong-Password';
FLUSH PRIVILEGES;
EXIT;
```

With this, you’ve completed the database installation and configuration, and you’re ready to install Gitea.

## Step 3: Install Gitea

First, create a new system user named `git`:

```bash
sudo adduser --system --shell /bin/bash --group --disabled-password --home /home/git git
```

Next, download Gitea and move it to the `/usr/bin` directory:

```bash
wget https://dl.gitea.com/gitea/1.19/gitea-1.22.3-linux-amd64
sudo mv gitea-1.22.3-linux-amd64 /usr/bin/gitea
```

Adjust the permissions for the Gitea binary:

```bash
sudo chmod 755 /usr/bin/gitea
```

Now, create the necessary directories for Gitea and set the correct permissions:

```bash
sudo mkdir -p /etc/gitea /var/lib/gitea/{custom,data,indexers,public,log}
sudo chown git:git /etc/gitea /var/lib/gitea/{custom,data,indexers,public,log}
sudo chmod 750 /var/lib/gitea/{data,indexers,log}
sudo chmod 770 /etc/gitea
```

Next, create the Gitea service file. Open a new file called `gitea.service` in `/etc/systemd/system`:

```bash
sudo nano /etc/systemd/system/gitea.service
```

Add the following content to the file:

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

Save the file (Ctrl + O, then Enter) and exit (Ctrl + X). Now, reload the systemd daemon and start the Gitea service:

```bash
sudo systemctl daemon-reload
sudo systemctl start gitea
```

To ensure the Gitea service starts on boot, run:

```bash
sudo systemctl enable gitea
```

## Step 4: Access the Gitea Web Interface

Open your web browser and navigate to `http://IP-address-of-your-server:3000`. You should see Gitea's initial configuration screen.

To use Gitea with a domain, set up a reverse proxy with your web server (Nginx, Apache, etc.). This allows you to access Gitea without needing to include a port number.

During the initial configuration, use the database credentials you set up earlier and configure the general settings. Click ‘Install Gitea,’ and you will be redirected to Gitea’s homepage where you can log in.
