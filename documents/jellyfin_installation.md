# Cyberdeck Media Server: Jellyfin Installation Guide

This guide will walk you through installing Jellyfin on your Raspberry Pi 5-based cyberdeck, turning it into a powerful offline multimedia server capable of streaming videos, music, and accessing documents and PDFs directly from your NVMe SSD.

---

## 🧰 Requirements

- Raspberry Pi 5 running Raspberry Pi OS (Bookworm 64-bit)
- Internet access (for installation phase)
- Mounted NVMe SSD at `/mnt/nvme`
- Your media (videos, music, books, documents) stored on NVMe
- Optional: UPS or power management board

---

## 🧼 Step 1: Clean Previous Web Servers (Optional)

If you previously used Lighttpd:

```bash
sudo systemctl stop lighttpd
sudo systemctl disable lighttpd
sudo apt remove --purge lighttpd -y
sudo apt autoremove -y
```

---

## 📦 Step 2: Add Jellyfin Repository

```bash
sudo apt update
sudo apt install -y apt-transport-https curl
curl -fsSL https://repo.jellyfin.org/debian/jellyfin_team.gpg.key | \
  sudo gpg --dearmor -o /usr/share/keyrings/jellyfin-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/jellyfin-archive-keyring.gpg] \
https://repo.jellyfin.org/debian bookworm main" | \
  sudo tee /etc/apt/sources.list.d/jellyfin.list
```

---

## 📥 Step 3: Install Jellyfin

```bash
sudo apt update
sudo apt install jellyfin -y
```

---

## 🚀 Step 4: Start and Enable Jellyfin Service

```bash
sudo systemctl enable jellyfin
sudo systemctl start jellyfin
```

---

## 🌐 Step 5: Access the Jellyfin Web Interface

Open your browser and go to:

```
http://<your-pi-ip>:8096
```

> Example: http://192.168.xx.xxx:8096

Complete the setup wizard:
- Create admin user
- Select language
- Add libraries:
  - `/mnt/nvme/Movies`
  - `/mnt/nvme/Music`
  - `/mnt/nvme/Books`
  - `/mnt/nvme/Documents`

---

## 🔐 Step 6: Permissions Fix (if needed)

Ensure Jellyfin can read your NVMe media:

```bash
sudo chown -R jellyfin:jellyfin /mnt/nvme
sudo chmod -R 755 /mnt/nvme
```

> If you prefer read-only access, skip `chown`.

---

## 🔁 Step 7: Auto-Mount NVMe on Boot

Find the UUID of your NVMe partition:
```bash
sudo blkid
```

Edit fstab:
```bash
sudo nano /etc/fstab
```

Add a line like:
```bash
UUID=xxxx-xxxx  /mnt/nvme  ext4  defaults,noatime  0  2
```

Save and reboot.

---

## 📱 Step 8: Use Jellyfin Remotely or via Mobile App

- Download the Jellyfin app for Android or iOS
- Connect to: `http://<your-pi-ip>:8096`

---

## ✅ You’re Done!

Your cyberdeck is now a full-featured multimedia hub:
- Watch movies
- Listen to music
- Read books and PDFs
- Stream over local network

Even in **offline/survival mode**, your Raspberry Pi-powered Jellyfin server will keep your media vault running.

---

Need help? You can always rerun `sudo systemctl restart jellyfin` if something fails.

Enjoy your offline cyberdeck media powerhouse! 💾⚡📺

