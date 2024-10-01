---
tags:
    - ubuntu
    - server
    - software
    - nextcloud
    - installation
---

# Setting Up Cloudflare and Nextcloud on Ubuntu Server

## Part 1: Setting Up Cloudflare

1. **Ensure Your Domain Uses Cloudflare DNS**
   - ![Insert Screenshot](#)

2. **[Cloudflare Zero Trust](https://one.dash.cloudflare.com)**

3. **Navigate to Network > Tunnels**.

4. **Create the tunnel and select Debian 64 Bit architecture.**

5. **Copy the Cloudflared Connector and install on Ubuntu 24.04 server**

6. **Configure Public Hostname**  
   (Further details needed)

---

## Part 2: Installing Nextcloud on Ubuntu Server 24.04

### Step 1: Initial Setup

1. **Install Necessary Packages:**

    ```bash
    sudo apt install -y eza redis-server build-essential unzip
    ```

2. **Replace `.bashrc`:**

    ```bash
    rm ~/.bashrc
    wget https://raw.githubusercontent.com/drewgrif/jag_dots/main/.bashrc_server
    mv ~/.bashrc_server ~/.bashrc
    bash
    ```

3. **Update and Upgrade:**

	```bash
    sudo apt update && sudo apt upgrade -y && sudo apt clean
	```

4. **Updating hostname**

	```shell
	sudo nano /etc/hostname
	```
	Add the subdomain from cloudflare `ex: my.justaguylinux.cloud`

	```shell
	sudo nano /etc/hosts
	```

	Add line `127.0.1.1	  ex: venetian  my.justaguylinux.cloud`

 
5. **Reboot the Server:**

	```bash
	sudo reboot
	```

6. **Log Back In.**

---

### Step 2: Downloading and Installing Nextcloud

1. **Download Nextcloud:**

    ```bash
    wget https://download.nextcloud.com/server/releases/latest.zip
    ```

2. **Install MariaDB:**

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
    

3. **Create Nextcloud Database:**

    ```bash
    sudo mariadb
    ```

    Inside the MariaDB shell:

    ```sql
    CREATE DATABASE nextcloud;
    GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY 'mypassword';
    FLUSH PRIVILEGES;
    ```

    Exit with `CTRL+D`.

---

### Step 3: Set Up Apache Webserver

1. **Install Required PHP Packages:**

    ```bash
    sudo apt install php php-apcu php-bcmath php-cli php-common php-curl php-gd php-gmp php-imagick php-intl php-mbstring php-mysql php-zip php-xml php-redis
    ```

2. **Enable Apache Modules:**

    ```bash
    sudo a2enmod dir env headers mime rewrite ssl
    ```
    
    ```bash
    sudo phpenmod bcmath gmp imagick intl redis
    ```

3. **Unzip Nextcloud:**

    ```bash
    unzip latest.zip
    ```
    
4. **Rename nextcloud to subdomain**
    
    ```bash
    mv nextcloud my.justaguylinux.cloud
    ```
5. **Change nextcloud ownership**

	```bash
	sudo chown -R www-data:www-data my.justaguylinux.cloud
	```

6. **Move nextcloud to apache**

	```bash
    sudo mv nextcloud /var/www
    ```
7. **Disable default apache site**
    
    ```bash
    sudo a2dissite 000-default.conf
    ```

8. **Configure Apache for Nextcloud:**

    ```bash
    sudo nano /etc/apache2/sites-available/my.justaguylinux.cloud.conf
    ```

    Add the following content:

    ```apache
    <VirtualHost *:80>
        DocumentRoot "/var/www/my.justaguylinux.cloud"
        ServerName my.justaguylinux.cloud

        <Directory "/var/www/my.justaguylinux.cloud/">
            Options MultiViews FollowSymlinks
            AllowOverride All
            Require all granted
        </Directory>

        TransferLog /var/log/apache2/my.justaguylinux.cloud_access.log
        ErrorLog /var/log/apache2/my.justaguylinux.cloud_error.log
    </VirtualHost>
    ```

9. **Enable Nextcloud Site and Restart Apache:**

    ```bash
    sudo a2ensite my.justaguylinux.cloud.conf
    sudo systemctl restart apache2
    ```

---

### Step 4: Adjust PHP Settings

1. **Edit PHP Configuration:**

    ```bash
    sudo micro /etc/php/8.3/apache2/php.ini
    ```

    !!! note "php.ini"
        Some of these will need changed others will need uncommnented and changed.

		memory_limit = 512M
		
		upload_max_filesize = 2G
		
		max_execution_time = 360
		
		post_max_size = 2G
		
		date.timezone = America/New_York
		
		opcache.enable = 1
		
		opcache.interned_strings_buffer = 32
		
		opcache.max_accelerated_files = 10000
		
		opcache.memory_consumption = 128
		
		opcache.save_comments = 1
		
		opcache.revalidate_freq = 1
		
2. **Enable acpu module**

	```bash
	sudo nano /etc/php/8.3/mods-available/apcu.ini
	```
	
	Add `apc.enable_cli=1`

3. **Restart Apache:**

    ```bash
    sudo systemctl restart apache2
    ```

---
