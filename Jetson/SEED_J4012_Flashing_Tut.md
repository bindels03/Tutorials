### **Work Instructions: Flashing Jetson ReComputer J4012 Using a Live USB and USB as Storage**  

These instructions guide you through setting up a **Live Ubuntu USB**, configuring an external **USB drive as the primary storage**, and preparing the system for flashing a **Jetson device**.

---

Guide for flashing Seed J40/30 Carrier Board using Ubuntu Live USB

Outline: 
   As a student I really want to have reliabiltiy with my computer, so when it came to choosing a main OS I went with the easy option Windows. Eventually I got the reComputer j4012 (and accidenatlly corrupted the jetpack install instantly) so I had to flash it. Only issue is to flash it I needed a non ARM host machine with Ubuntu installed. I didn't want to dual boot ubuntu and windows since I only have one drive in my computer right now and I still wanted to have windows for ease of use. My workaround was to flash the board using a live version of Ubunut running off of a USB. As a bonus since this setup is modular as long you save the flashing files to the usb you only have to set this oe once and can jsut boot into Ubunut and mount the usb and flash from there. 

Steps : 

0: back up windows - make live ubuntu usb give link 
1: Turn off Secure boot and boot into Ubuntu 
2: mount and foirmat extra usb
3:download files
4: copy to usb and change permissions
5: open root terminal in usb and start flashing 



## **Step 1: Prepare the Live USB**
1. **Download Ubuntu ISO**  
   - Obtain the latest **Ubuntu Desktop ISO** (recommended version: **Ubuntu 20.04+**).
  
2. **Create the Live USB**  
   - Use **Rufus** (Windows) or `dd` (Linux/macOS) to write the ISO to a **USB flash drive** (minimum **8GB** recommended).  
   - If using Rufus:  
     - Select **MBR** partition scheme.  
     - Use **FAT32** for compatibility.  
     - Enable **Persistent Storage** if you want to keep changes after reboot.  

3. **Boot from the Live USB**  
   - Insert the Live USB into the **host PC**.  
   - Enter BIOS/boot menu and boot from the USB.  
   - Select **"Try Ubuntu"** (do not install).  

---

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
