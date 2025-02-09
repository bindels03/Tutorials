
**Guide for Flashing Seed J401 Carrier Board using Ubuntu Live USB**
=========================================================================
---
### Background:

Since I am a student I really need my compputer to be reliable. I initially chose Windows as my main OS on my desktop for its ease of use. However, after getting the reComputer J401 (and accidentally corrupting the JetPack installation lol) I needed a way to flash it. The issue? Flashing requires a non-ARM host machine with Ubuntu installed, and I didn't want to dual-boot Ubuntu alongside Windows due to my limited storage.

### Solution:

I found a workaround by flashing the board using a Live USB session of Ubuntu. This modular setup allows me to save  the flashing files to the USB, meaning I only have to set this up once. In the future, I can simply boot into Ubuntu, mount the USB, and flash the Jetson without additional setup.

### Steps : 


NOTES::

USE AT LEAST 64 GB USB
IT WILL TAKE LONGER THIS WAY SLOW WRITE TIMES
BUT ONCE SET UP JUST NEED TO WAIT TO FLASH 

0: back up windows - make live ubuntu usb give link 
1: Turn off Secure boot and boot into Ubuntu 
2: mount and foirmat extra usb
å3:download files
4: copy to usb and change permissions
5: open root terminal in usb and start flashing 

tip for revovery mode use pin jumpiner 

1 umount usb use diskl untulity to figure out which one is it
2 then format it
3 re mount in easy spot 
4.5 change download dir in firefox to new moutned folder and change permissions 
4 download files once its mounted ( i do mnt/jetson)
5 force revocery mode and plug in
6. extract files or whtv follow set s from see but then just use the usb as working dir
7 flash 
8 

---
## **Step 0: Prepare the Live USB, Disable Secure Boot, Backup Windows (Optional)**
1. **Create the Live USB**  
   - Download Ubuntu 20.0.4 ISO: 
     - https://ubuntu.com/download/alternative-downloads
   - Follow Offical Ubuntu Guide for creating live USB:
        - https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview
  2. **Backup Windows (Optional)**  
   - Optional Step to Create Windows Recovery USB if you're worried about corrupting your install - has not happened to me but you never know
     - https://support.microsoft.com/en-us/windows/recovery-drive-abb4691b-5324-6d4a-8766-73fab304c246
    - Disable BitLocker if you have it - if you boot into another OS and then back into windows BitLocker will lock all your files and then thats a big hassle

3. **Disable Secure Boot**  
   - Restart your PC and go into the BIOS 
   - Disable Secure Boot 

---

## **Step 1: Boot from Live USB**


Insert the Live USB into the host PC.

Restart the PC and press the appropriate key (e.g., F12, ESC, DEL) to enter the Boot Menu.

Select the USB device from the boot options and press Enter.

When prompted, select "Try Ubuntu" (do not install Ubuntu).


## **Step 2: Set Up External USB as the Storage Device**
1. **Insert the USB drive** (this will act as the working storage).  
2. **Find the USB device name**:  
   ```bash
   lsblk
   ```
   - Look for a drive like `/dev/sdb` (size should match your USB drive).  

3. **Format the USB drive**:  
   - If you need a fresh setup, format it as **ext4**:  
     ```bash
     sudo mkfs.ext4 /dev/sdX
     ```
     *(Replace `sdX` with the correct drive name, e.g., `sdb`.)*  

4. **Mount the USB drive for use**:  
   ```bash
   sudo mkdir -p /mnt/jetson
   sudo mount /dev/sdX /mnt/jetson
   ```
   *(Replace `sdX` with your actual device name.)*  

5. **Give full permissions**:  
   ```bash
   sudo chmod -R 777 /mnt/jetson
   ```

---

## **Step 3: Copy Jetson Flashing Files to USB Storage**
1. **Check the official tutorial for the latest download links:**  
   🔗 [Seeed Studio: Flash JetPack on reComputer J4012](https://wiki.seeedstudio.com/reComputer_J4012_Flash_Jetpack/)
2. **Download the necessary Jetson files to the Live USB session.**  
3. **Move the extracted files to the mounted USB drive** (this USB will serve as the working directory for completing the flashing process):  
   ```bash
   sudo cp -r /path/to/Jetson_Files /mnt/jetson/
   ```
4. **Verify ownership and permissions**:  
   ```bash
   sudo chown -R ubuntu:ubuntu /mnt/jetson
   sudo chmod -R 777 /mnt/jetson
   ```

---

## **Step 4: Grant Root Access for Flashing**
1. **Open a root shell**:  
   ```bash
   sudo -i
   ```
   *(This ensures all commands have the necessary privileges.)*  
2. **Navigate to the working directory**:  
   ```bash
   cd /mnt/jetson/Linux_for_Tegra/
   ```

---
extract took 2 hours for me lots of files and slow write speed 

## **Step 5: Continue with Flashing Instructions**
To proceed with flashing the Jetson device, follow the official instructions here:  
🔗 [Seeed Studio: Flash JetPack on reComputer J4012](https://wiki.seeedstudio.com/reComputer_J4012_Flash_Jetpack/)

📌 **Note:** All commands in the official tutorial should be run from the **mounted USB directory** (`/mnt/jetson`), where the JetPack files were moved earlier.

---

### **Notes:**
- Using **persistent storage** on a Live USB can help, but a **full install on a USB drive** is more reliable.  
- **USB 3.0+ is strongly recommended** for better write speeds.  
- **If using a slow USB**, expect `apply_binaries.sh` and `syncing system.img` to take **significantly longer** than on an SSD.  
- When troubleshooting, always check `iotop` and `top` to monitor **disk activity and CPU usage**.

---

### **Future Use:**
- These steps remain the same for **future JetPack versions**, with the only variation being the **flashing script**.  
- Keeping an **Ubuntu full installation on a USB drive** makes the setup more stable for future flashing.

---

### **Summary:**
✔ **Set up Live USB**  
✔ **Mount and prepare USB as storage**  
✔ **Copy and apply permissions to Jetson files**  
✔ **Grant root access for flashing**  
✔ **Follow the official Seeed Studio guide to flash the device**  

Let me know if you want any refinements! 🚀
https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview
