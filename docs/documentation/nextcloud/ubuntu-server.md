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
   ![2024-09-30_16-55](https://github.com/user-attachments/assets/25d76502-6cfe-4c63-8c04-1a287376c9f8)

2. **[Cloudflare Zero Trust](https://one.dash.cloudflare.com)**

3. **Navigate to Network > Tunnels**.

4. **Create the tunnel and select Debian 64 Bit architecture.**

5. **Copy the Cloudflared Connector and install on Ubuntu 24.04 server**

6. **Configure Public Hostname**  
 ![2024-09-30_17-18](https://github.com/user-attachments/assets/a1ce0a62-b72d-4d92-99b7-fb2f38f6cda7)

---

## Part 2: Installing Nextcloud on Ubuntu Server 24.04

### Step 1: Initial Setup

1. **Install Necessary Packages:**

    ```bash
    sudo apt install -y eza redis-server build-essential unzip libmagickwand-dev librsvg2-dev
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
    sudo mv my.justaguylinux.cloud /var/www
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
    sudo nano /etc/php/8.3/apache2/php.ini
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
	
	Add to the file: `apc.enable_cli=1`

3. **Restart Apache:**

    ```bash
    sudo systemctl restart apache2
    ```

---

### Step 5: Nextcloud via Web Browser

1. **Install via Web:  https://my.justaguylinux.cloud**
![2024-10-01_13-50](https://github.com/user-attachments/assets/fa63b0bb-6f52-49f3-8341-11a3ba5df3f9)

2. **Install recommended applications**

---

## Part 3: Dealing with Warnings

1. **Change config.php permissions**

	```bash
	sudo chmod 660 /var/www/my.justaguylinux.cloud/config/config.php
	```

2. **Detected some missing optional indices.**

	Make occ executable: 
	
	```bash
	sudo chmod +x /var/www/my.justaguylinux.cloud/occ
	```
	
	Then add missing indices:
	
	```bash
	sudo /var/www/my.justaguylinux.cloud/occ db:add-missing-indices
	```
	
	Check to see if error is no longer
	
3. **One or more mimetype migrations are available**

	```bash
	sudo /var/www/my.justaguylinux.cloud/occ maintenance:repair --include-expensive
	```
	
4.  **The following warnings**
	
	- Server has no maintenance window start time configured.

	- The database is used for transactional file locking.

	- No memory cache has been configured.

	- Your installation has no default phone region set.

	- You have not set or verified your email server configuration yet.
	
	```bash
	sudo nano /var/www/my.justaguylinux.cloud/config/config.php
	```
	**Remove:** `maintenance => false,`
	
	**Add:**
	
	```php
    'mail_from_address' => 'nextcloud',
	'mail_smtpmode' => 'smtp',
	'mail_sendmailmode' => 'smtp',
	'mail_domain' => 'gmail.com',
	'mail_smtphost' => 'smtp.gmail.com',
	'mail_smtpport' => '587',
	'mail_smtpauth' => 1,
	'mail_smtpname' => 'nextcloud@gmail.com',
	'mail_smtppassword' => 'app-specific-pw',
	'maintenance_window_start' => 1,
	'memcache.local' => '\\OC\\Memcache\\APCu',
	'memcache.distributed' => '\\OC\\Memcache\\Redis',
	'memcache.locking' => '\\OC\\Memcache\\Redis',
	'redis' => 
	array (
	'host' => 'localhost',
	'port' => 6379,
	),
	'default_phone_region' => 'US',
	'overwriteprotocol' => 'https',
	```

5. **Some headers are not set correctly on your instance - The `Strict-Transport-Security` HTTP header is not set (should be at least `15552000` seconds). **

	```bash
	sudo nano /etc/apache2/sites-available/my.justaguylinux.cloud.conf
	```
	
	**Add after </Directory>**
	
	```ini
	<IfModule mod_headers.c>
      Header always set Strict-Transport-Security "max-age=15552000; in>
    </IfModule>
    ```
    **Restart apache2**
    
    ```bash
    sudo systemctl restart apache2
    ```

6. **Array:  Not a warning but good idea**

	```bash
	sudo nano /var/www/my.justaguylinux.cloud/config/config.php
	```

	```bash
	'trusted_domains' => 
	array (
    0 => '192.168.254.80',
    1 => 'venetian',
    2 => 'my.justaguylinux.cloud',
	),
	```
