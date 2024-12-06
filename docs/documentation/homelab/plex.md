---
tags:
    - plex
    - software
    - homelab
    - installation
---
# 📺 Plex 

## 📖 Introduction

**This guide assumes you are starting with a headless Debian server.** It provides step-by-step instructions for installing Plex Media Server, including optional enhancements and handling repository keys.

## 🛠️ Installation Steps

### 1. Install Required Packages

First, install the necessary packages for adding repositories and handling keys:

```shell
sudo apt install apt-transport-https curl wget sudo gnupg2 -y
```

### 2. Add Plex Repository

Add the Plex Media Server repository to your system:

```shell
echo "deb https://downloads.plex.tv/repo/deb public main" | sudo tee /etc/apt/sources.list.d/plexmediaserver.list
```

### 3. Add Plex GPG Key

Download and add the Plex GPG key for package verification:

```shell
curl https://downloads.plex.tv/plex-keys/PlexSign.key | sudo apt-key add -
```

### 4. Update Package List

Update your package list to include Plex:

```shell
sudo apt update
```

**Note:** You might encounter an [apt-key deprecation warning](https://askubuntu.com/questions/1398344/apt-key-deprecation-warning-when-updating-system) during the update. To resolve this, copy the trusted keys:

```shell
cd /etc/apt
sudo cp trusted.gpg trusted.gpg.d
```

After copying the keys, run the update again:

```shell
sudo apt update
```

### 5. Install Plex Media Server

Now, install Plex Media Server:

```shell
sudo apt install plexmediaserver
```

### 6. Access Plex Media Server

Find your server’s IP address with:

```shell
ip -f inet address | grep inet | grep -v 'lo$' | cut -d ' ' -f 6,13 && curl ifconfig.me && echo ' external ip'"

```

Then, access the Plex web interface by navigating to:

```
http://myinternal_ip:32400/web
```

Replace `myinternal_ip` with the actual IP address of your server.

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="justaguylinux" data-description="Support me on Buy me a coffee!" data-message="" data-color="#FF5F5F" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
