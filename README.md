# AirPrint iOS TV Box Solution

Transform a $10 TV Box into a 24/7 AirPrint server! Complete guide to install Linux on old TV Box and enable AirPrint support for non-AirPrint printers like HP LaserJet 2055. Now all iOS devices in your home can print wirelessly to old printers.

## ✨ Features

- ♻️ **Repurposes** $10 old TV Box into useful hardware
- 🖨️ **Adds AirPrint** support to HP LaserJet 2055 (and similar printers)
- 📱 **24/7 availability** for all iOS devices in the network
- 💰 **Costs virtually nothing** - uses existing hardware
- 🐧 **Linux-based** reliable solution
- 🔧 **Step-by-step** easy to follow guide

## 🛠️ Tested Hardware

- **TV Box**: Generic Android TV Box (~$10)
- **Printer**: HP LaserJet 2055dn (non-AirPrint)
- **iOS Devices**: iPhone, iPad (all versions)
- **Result**: Perfect 24/7 wireless printing

## 💰 Cost Comparison

| Solution | Cost | Setup Complexity | Reliability |
|----------|------|------------------|-------------|
| **This TV Box Method** | ~$10 | Medium | Excellent |
| New AirPrint Printer | $200+ | Easy | Excellent |
| Raspberry Pi Solution | $50+ | Complex | Good |
| Commercial Print Server | $100+ | Easy | Good |

## 📖 Guide

🧠 Overview
Goal: Turn your old Android TV box (Amlogic S905W) into a small Linux print server accessible from any Apple device via AirPrint.
You’ll need:
- Amlogic S905W TV box
-	USB flash drive (≥ 8 GB)
-	PC with Linux/macOS/Windows
-	Armbian image for S905W
-	USB keyboard + monitor (optional but helpful)
 
🪛 1. Flash and boot Armbian on Amlogic S905W

Step 1. Prepare the Armbian image
1.	Download the latest Armbian image for Amlogic S905W
→ e.g. from: https://github.com/ophub/amlogic-s9xxx-armbian
Example file:
Armbian_24.8.0_amlogic_s905w_bookworm_6.1.49_server_2024.img.xz
2.	Write it to your USB flash drive (8–16 GB):
-	On Linux/macOS:
-	sudo dd if=Armbian_24.8.0_amlogic_s905w_bookworm_6.1.49_server_2024.img of=/dev/sdX bs=1M status=progress
-	sync
(replace /dev/sdX with your actual USB device)
-	On Windows: use balenaEtcher or Rufus.
 
Step 2. Boot the TV box into recovery mode
1.	Unplug power from the TV box.
2.	Insert the flashed USB stick into a USB port.
3.	Press and hold the reset button (usually inside the AV jack) for 15 seconds.
4.	While holding it, plug the power cable back in.
5.	Release the button once you see the recovery menu or the Armbian logo.
If all goes well, the box will boot from USB and start Armbian initialization.
 
Step 3. First-time setup
1.	Login when prompted:
2.	login: root
3.	password: 1234
4.	Change the password and create a new user when asked.
5.	Verify network connectivity:
6.	ip a
7.	ping google.com
 
🧩 2. Install and configure CUPS

CUPS (Common Unix Printing System) lets your TV box act as a print server.

Step 1 — Install required packages

Update your package list and install CUPS:
```bash
sudo apt update
sudo apt install cups -y
```

Step 2. Allow admin via web interface
```bash
sudo usermod -aG lpadmin $USER
sudo systemctl enable cups
sudo systemctl start cups
```

Step 3. Enable remote access (edit config)
```bash
sudo nano /etc/cups/cupsd.conf
```

Find and modify/add the following lines in /etc/cups/cupsd.conf:

Listen *:631
Port 631
Browsing On
Allow @LOCAL


Also, inside <Location /> and <Location /admin> blocks, add:

Order allow,deny
Allow @LOCAL


Restart CUPS:
```bash
sudo systemctl restart cups
```

Now open CUPS web interface from your PC:

http://<TVBOX_IP>:631


→ You can add and test printers here.  
🍎 3. Enable AirPrint (AirPrint = CUPS + Avahi)  
Step 1. Install Avahi
```bash
sudo apt install avahi-daemon avahi-discover -y
```
Step 2. Verify it’s running
```bash
sudo systemctl status avahi-daemon
```
Should show active (running).
Step 3. Restart services
```bash
sudo systemctl restart cups avahi-daemon
```
Now your printer should automatically appear on all iPhones/iPads/Macs under “AirPrint printers” — no extra drivers needed.
 
⚙️ 4. Optional tuning
•	Make USB boot permanent:
```bash
•	sudo /root/install-aml.sh
```
(Writes Armbian to internal eMMC — optional but faster.)
•	Check printer list:
```bash
•	lpstat -p -d
```
•	Reboot cleanly:
```bash
•	sudo reboot
```
✅ Result
You now have:
•	Armbian running on Amlogic S905W
•	Full Linux print server (CUPS)
•	Automatic AirPrint discovery for Apple devices
Your once-idle TV box just got a second life as a silent, always-on print server.

<img width="453" height="702" alt="image" src="https://github.com/user-attachments/assets/7d68d470-e7ae-4720-a138-c6113167391e" />


## 🤝 Contributing
Feel free to submit issues and enhancement requests!
