---
tags:
    - software
    - homelab
    - jitsi
    - documentation
---

# 🎥 Jitsi

> This installation is how I installed Jitsi on a Linode server. 
`Currently using the Dedicated 4 GB RAM | 2 CPU |  80 GB Disk for $36/mo.`

> [Linode - Sign up and receive a $100, 60-day credit](https://www.linode.com/lp/refer/?r=06e69fe957f6b439e07b808b83b93da9f29f1f2c){:target="_blank"}

## 🌐 Domain Setup
You are using a Linode dedicated with 4 cores and 8 GB of RAM, with a static IP and RDNS configured. Ensure you are using Linode DNS.

## 🛠️ Update and Install Necessary Packages

First, update your package list and install necessary transport packages:

```bash
sudo apt update
sudo apt install apt-transport-https gnupg2 nginx-full curl
```

Ensure the `universe` repository is added:

```bash
sudo apt-add-repository universe
```

Set the hostname for your server:

```bash
sudo hostnamectl set-hostname jitsi.[DOMAIN_NAME]
```

Edit the `/etc/hosts` file to map your static IP to the hostname:

```bash
sudo nano /etc/hosts
```

Add the following lines, replacing `x.x.x.x` with your static IP:

```
127.0.0.1
x.x.x.x jitsi.[DOMAIN_NAME]
```

## 🔐 Configure Firewall

Allow necessary ports through the firewall:

```bash
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 3478/udp
sudo ufw allow 5349/tcp
sudo ufw allow 10000/udp
```

Check the firewall status and enable it if necessary:

```bash
sudo ufw status
sudo ufw enable
```

## 🚀 Installing Jitsi

### 📦 Add Prosody Repository

Add the Prosody repository and install `lua5.2`:

```bash
sudo curl -sL https://prosody.im/files/prosody-debian-packages.key -o /etc/apt/keyrings/prosody-debian-packages.key
echo "deb [signed-by=/etc/apt/keyrings/prosody-debian-packages.key] http://packages.prosody.im/debian jammy main" | sudo tee /etc/apt/sources.list.d/prosody-debian-packages.list
sudo apt install lua5.2
```

### 🔗 Add Jitsi Repository

Add the Jitsi repository and install Jitsi Meet:

```bash
curl -sL https://download.jitsi.org/jitsi-key.gpg.key | sudo sh -c 'gpg --dearmor > /usr/share/keyrings/jitsi-keyring.gpg'
echo "deb [signed-by=/usr/share/keyrings/jitsi-keyring.gpg] https://download.jitsi.org stable/" | sudo tee /etc/apt/sources.list.d/jitsi-stable.list
```

Update the package list and install Jitsi Meet:

```bash
sudo apt update
sudo apt install jitsi-meet
```

**Note:** Ensure you use the correct hostname and configure Let's Encrypt for SSL.

## 🔒 Securing Room Creation

### 🛡️ Configure Prosody

Edit the Prosody configuration file:

```bash
sudo nano /etc/prosody/conf.avail/jitsi.[DOMAIN_NAME].cfg.lua
```

Enable authentication by modifying the `VirtualHost` block:

```lua
VirtualHost "jitsi.[DOMAIN_NAME]"
    authentication = "internal_hashed"
```

Add the following to the end of the file:

```lua
VirtualHost "guest.jitsi.[DOMAIN_NAME]"
    authentication = "anonymous"
    c2s_require_encryption = false
    modules_enabled = {
        "bosh";
        "ping";
        "pubsub";
        "speakerstats";
        "turncredentials";
        "conference_duration";
    }
```

### ⚙️ Update Jitsi Configuration

Edit the Jitsi Meet configuration file:

```bash
sudo nano /etc/jitsi/meet/jitsi.[DOMAIN_NAME]-config.js
```

Search for `anonymousdomain:` and update it:

```javascript
anonymousdomain: 'guest.jitsi.[DOMAIN_NAME]',
```

### 🧩 Configure Jicofo

Edit the Jicofo configuration file:

```bash
sudo nano /etc/jitsi/jicofo/sip-communicator.properties
```

Update the `auth.URL` property:

```properties
org.jitsi.jicofo.auth.URL=XMPP:jitsi.[DOMAIN_NAME]
```

## 📝 Register Names

Register the Jitsi user:

```bash
sudo prosodyctl register user jitsi.[DOMAIN_NAME] password
```

Use `unregister` to remove a user if needed.

## 🔄 Restart Services

Restart the necessary services:

```bash
sudo systemctl restart prosody.service jicofo.service jitsi-videobridge2.service
```

**Note:** Ensure that RDNS for your Linode is set to `jitsi.[DOMAIN_NAME]`.
```

In this updated version, emojis are placed directly before the header text to enhance visual appeal and aid in navigation. Adjust the emojis based on your preference or context if needed!
