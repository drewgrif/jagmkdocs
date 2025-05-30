---
tags:
    - mount
    - software
    - documentation
    - samba
    - file manager
    - thunar
    - fstab
---
# 🗂️ How to Mount SMB Shares

This guide shows you how to mount SMB/CIFS network shares on Linux with proper permissions. 🚀

## 🚀 Method 1: Basic Mount

### 📁 Create a mount point
```bash
sudo mkdir /mnt/myshare
```

### 🔗 Mount the share
```bash
sudo mount -t cifs //server-ip/share /mnt/myshare -o username=youruser,password=yourpass,uid=1000,gid=1000,file_mode=0664,dir_mode=0775
```

### ✅ Test it works
```bash
touch /mnt/myshare/test.txt
mkdir /mnt/myshare/testfolder
```

## ⚡ Method 2: Permanent Mount (fstab)

### 📝 Add to fstab for automatic mounting
```bash
sudo nano /etc/fstab
```

### ➕ Add this line
```
//server-ip/share /mnt/myshare cifs username=youruser,password=yourpass,uid=1000,gid=1000,file_mode=0664,dir_mode=0775 0 0
```

### 🔄 Mount all fstab entries
```bash
sudo mount -a
```

## 🔐 Method 3: Secure Credentials

### 📄 Create credentials file
```bash
sudo nano /etc/cifs-credentials
```

### 🔑 Add your login info
```
username=youruser
password=yourpass
```

### 🛡️ Secure the file
```bash
sudo chmod 600 /etc/cifs-credentials
```

### 🎯 Use it in fstab
```
//server-ip/share /mnt/myshare cifs credentials=/etc/cifs-credentials,uid=1000,gid=1000,file_mode=0664,dir_mode=0775 0 0
```

## 🛠️ Common Options

- `file_mode=0664` - Files are readable/writable 📄
- `dir_mode=0775` - Folders are accessible 📂
- `uid=1000` - Your user ID (check with `id -u`) 👤
- `gid=1000` - Your group ID (check with `id -g`) 👥
- `vers=3.0` - SMB version (try 2.0 if 3.0 fails) 🔢

## 🎯 Important for File Managers

When using Thunar or other file managers, navigate to your mount point (like `/home/user/mount`) instead of using `smb://` URLs. This ensures applications can properly open files from the network share.

- ✅ Use: `/home/user/mount/file.txt`
- ❌ Avoid: `smb://server/share/file.txt`

## 🔌 Unmount

```bash
sudo umount /mnt/myshare
```
