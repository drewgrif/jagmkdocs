---
tags:
    - debian
    - software
    - nextcloud
    - installation
---
### Preparing the Drive for Nextcloud

#### Identify the Intended Drive

Use `lsblk` to identify the drive you want to use for Nextcloud. In this example, we’ll assume the drive is `/dev/sda`.

#### Partitioning the Drive

1. **Install `parted`**

    ```shell
    sudo apt install -y parted
    ```

2. **Create a GPT Partition Table**

    ```shell
    sudo parted /dev/sda mklabel gpt
    ```

3. **Create a Partition**

    ```shell
    sudo parted /dev/sda mkpart primary ext4 0% 100%
    ```

4. **(Optional) Name the Partition**

    ```shell
    sudo parted /dev/sda name 1 my-data
    ```

5. **Format the Partition**

    ```shell
    sudo mkfs.ext4 /dev/sda1
    ```

#### Mount the Partition

1. **Create a Mount Point**

    ```shell
    sudo mkdir /nextcloud-data
    ```

2. **Mount the Partition**

    ```shell
    sudo mount /dev/sda1 /nextcloud-data
    ```

3. **Add to `/etc/fstab` for Automatic Mounting**

    ```shell
    echo '/dev/sda1 /nextcloud-data ext4 defaults 0 0' | sudo tee -a /etc/fstab
    ```

4. **Set Proper Permissions**

    ```shell
    sudo chown www-data:www-data -R /nextcloud-data
    sudo chmod 0750 -R /nextcloud-data
    ```

#### Final Steps

Once these steps are completed, you can use the USB drive as the data drive during the Nextcloud installation.


