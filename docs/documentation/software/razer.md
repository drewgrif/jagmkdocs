To adjust the mouse's FPS (frames per second) or polling rate in Debian 12, you can modify the mouse polling rate. This determines how often the mouse reports its position to the operating system. Here's how to do it:



### 1. **Change the Polling Rate**
   If your mouse supports it, you can adjust the polling rate by changing the USB module's options or using a configuration tool.

   #### **Option A: For USB Mice**
   - Add a new configuration to adjust the USB polling rate:
     ```bash
     sudo nano /etc/modprobe.d/usbmouse.conf
     ```
   - Add the following line to set the polling interval (in milliseconds):
     ```
     options usbhid mousepoll=1
     ```
     The value `1` corresponds to a polling rate of 1000 Hz (1 ms). Adjust it as needed:
     - `2` = 500 Hz
     - `4` = 250 Hz
     - `8` = 125 Hz

   - Update the initramfs and reboot:
     ```bash
     sudo update-initramfs -u
     sudo reboot
     ```

   #### **Option B: Use Custom Drivers**
	Since I am using the:
	- Razer BlackWindow 2019
	- Razer Viper
	
	Review: [https://polychromatic.app/download/debian/](Polychromatic - Razer). 
	Check the openrazer-meta within the Debian Repos.
	
	

---

