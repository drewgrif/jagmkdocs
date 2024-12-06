---
tags:
    - debian
    - software
    - nextcloud
    - installation
---

## Installing Nextcloud on Debian

### After Debian Minimal Install

1. **Install Necessary Packages:**

    ```shell
    sudo apt install -y exa micro neofetch zram-tools wget unzip curl
    ```

2. **Change zram Configuration:**

    ```shell
    sudo micro /etc/default/zramswap
    ```

3. **Replace `.bashrc`:**

    ```shell
    rm .bashrc
    wget https://raw.githubusercontent.com/drewgrif/dotfiles/main/.bashrc
    bash
    ```

### Downloading and Installing Nextcloud

1. **Download Nextcloud:**

    ```shell
    wget https://download.nextcloud.com/server/releases/latest.zip
    ```

2. **Install MariaDB:**

    ```shell
    sudo apt install -y mariadb-server
    sudo mysql_secure_installation
    ```

3. **Create Nextcloud Database:**

    ```shell
    sudo mariadb
    ```

    Inside the MariaDB shell:

    ```sql
    CREATE DATABASE nextcloud;
    GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY 'mypassword';
    FLUSH PRIVILEGES;
    ```

    Exit the MariaDB shell with `CTRL+D`.

4. **Set Up Apache Webserver:**

    - **Install Required Packages:**

        ```shell
        sudo apt install php php-apcu php-bcmath php-cli php-common php-curl php-gd php-gmp php-imagick php-intl php-mbstring php-mysql php-zip php-xml
        ```

    - **Enable Apache Modules:**

        ```shell
        sudo a2enmod dir env headers mime rewrite ssl
        sudo phpenmod bcmath gmp imagick intl
        ```

    - **Unzip and Move Nextcloud:**

        ```shell
        unzip latest.zip
        sudo chown -R www-data:www-data nextcloud
        sudo mv nextcloud /var/www
        sudo a2dissite 000-default.conf
        ```

    - **Configure Apache for Nextcloud:**

        ```shell
        sudo micro /etc/apache2/sites-available/nextcloud.conf
        ```

        Add the following contents to `nextcloud.conf`:

        ```apache
        <VirtualHost *:80>
            DocumentRoot "/var/www/nextcloud"
            ServerName nextcloud

            <Directory "/var/www/nextcloud/">
                Options MultiViews FollowSymlinks
                AllowOverride All
                Order allow,deny
                Allow from all
            </Directory>

            TransferLog /var/log/apache2/nextcloud_access.log
            ErrorLog /var/log/apache2/nextcloud_error.log
        </VirtualHost>
        ```

    - **Enable Nextcloud Site and Restart Apache:**

        ```shell
        sudo a2ensite nextcloud.conf
        sudo systemctl restart apache2
        ```

5. **Adjust PHP Settings (for Debian 12):**

    ```shell
    sudo micro /etc/php/8.2/apache2/php.ini
    ```

    Update the following parameters:

    ```ini
    memory_limit = 512M
    upload_max_filesize = 16G
    max_execution_time = 360
    post_max_size = 16G
    date.timezone = America/New_York
    opcache.enable=1
    opcache.interned_strings_buffer=32
    opcache.max_accelerated_files=10000
    opcache.memory_consumption=128
    opcache.save_comments=1
    opcache.revalidate_freq=1
    ```

6. **Restart Apache:**

    ```shell
    sudo systemctl restart apache2
    ```

---

For additional configuration related to using an external USB drive, see [02 Using an external USB drive](#).
sudo mysql_secure_installation
```

#### Create Nextcloud Database

```shell
sudo mariadb
```

```shell
CREATE DATABASE nextcloud;
```
```shell
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY 'mypassword';
```
```shell
FLUSH PRIVILEGES;
```
CTRL+D to exit

#### Apache Webserver Setup

Installing the required packages to support Apache:

``` shell
sudo apt install php php-apcu php-bcmath php-cli php-common php-curl php-gd php-gmp php-imagick php-intl php-mbstring php-mysql php-zip php-xml
```
```shell
sudo a2enmod dir env headers mime rewrite ssl
```
```shell
sudo phpenmod bcmath gmp imagick intl
```

#### Unzip and move nextcloud

```shell
unzip latest.zip
sudo chown -R www-data:www-data nextcloud
sudo mv nextcloud.learnlinux.cloud /var/www
sudo a2dissite 000-default.conf
```
```shell
sudo micro /etc/apache2/sites-available/nextcloud.conf
```
Contents of nextcloud.conf file.
```
<VirtualHost *:80>
    DocumentRoot "/var/www/nextcloud"
    ServerName nextcloud

    <Directory "/var/www/nextcloud/">
        Options MultiViews FollowSymlinks
        AllowOverride All
        Order allow,deny
        Allow from all
   </Directory>

   TransferLog /var/log/apache2/nextcloud_access.log
   ErrorLog /var/log/apache2/nextcloud_error.log

</VirtualHost>
```

```shell
sudo a2ensite nextcloud.conf
```

As of Debian 12
``` shell
sudo micro /etc/php/8.2/apache2/php.ini
```

Adjust the following parameters:

* memory_limit = 512M
* upload_max_filesize = 16G
* max_execution_time = 360
* post_max_size = 16G
* date.timezone = America/New_York
* opcache.enable=1
* opcache.interned_strings_buffer=32
* opcache.max_accelerated_files=10000
* opcache.memory_consumption=128
* opcache.save_comments=1
* opcache.revalidate_freq=1

```shell
sudo systemctl restart apache2
```

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="justaguylinux" data-description="Support me on Buy me a coffee!" data-message="" data-color="#FF5F5F" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
