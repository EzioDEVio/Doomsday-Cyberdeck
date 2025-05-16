# Offline Wikipedia Setup on Cyberdeck: Kiwix Installation Guide

This README is a full step-by-step guide to installing, configuring, and troubleshooting Kiwix on a Raspberry Pi 5-based cyberdeck running Raspberry Pi OS (Bookworm, 64-bit). It enables offline access to Wikipedia, Wikibooks, Wiktionary, and other `.zim` knowledge libraries.

---

## 🧰 Prerequisites

- Raspberry Pi 5 with Raspberry Pi OS Bookworm (64-bit)
- 128GB+ SD card or NVMe USB drive with sufficient space
- SSH access from a Windows PC
- Internet access for the installation phase
- At least one `.zim` file (e.g., Wikipedia)

---

## 📦 Step 1: Install Kiwix Command Line Tools

```bash
sudo apt update
sudo apt install kiwix-tools -y
```

This includes:
- `kiwix-serve` (for serving .zim files via browser)
- `kiwix-manage` (for managing .zim libraries)

---

## 📁 Step 2: Create a ZIM Directory

```bash
mkdir -p ~/kiwix/zims
```

This is where you will store `.zim` files.

---

## 🔄 Step 3: Transfer `.zim` Files from Windows

### ✅ Option A: Use SCP from PowerShell

```powershell
scp "C:\Path\To\Your\wikipedia_en_all_maxi_2024-01.zim" ezio@<pi-ip>:/home/ezio/kiwix/zims/
```

Replace `<pi-ip>` with your Pi’s IP (e.g. `192.168.254.152`), and use your Pi password when prompted.

### ✅ Option B: Use WinSCP (GUI Method)

1. Download WinSCP: https://winscp.net/
2. Connect to your Pi using:
   - Host: `192.168.254.152`
   - Username: `ezio`
   - Protocol: SCP
3. Drag `.zim` files into `/home/ezio/kiwix/zims/`

---

## 🔎 Step 4: Verify Transfer

```bash
ls -lh ~/kiwix/zims
```

Make sure the file size matches the original on your PC (Wikipedia `.zim` ~90GB).

To get real-time progress during future transfers:

```bash
rsync -ah --progress <source> <destination>
```

Example:
```bash
rsync -ah --progress "/media/usb/WIKIPEDIA AND SOFTWARES/wikipedia_en_all_maxi_2024-01.zim" ~/kiwix/zims/
```

---

## 🚀 Step 5: Launch Kiwix Server

```bash
kiwix-serve --port=80801~/kiwix/zims/wikipedia_en_all_maxi_2024-01.zim
```

Access it from any browser on the same network:
```
http://<pi-ip>:8080
```
Example:
```
http://192.168.254.152:8081
```

---

## 📚 Step 6: Add Multiple ZIMs (Optional)

If you have multiple `.zim` files:
```bash
kiwix-manage ~/kiwix/library.xml add ~/kiwix/zims/*.zim
```

Then serve them all from one interface:
```bash
kiwix-serve --port=8080 --library ~/kiwix/library.xml
```

This creates a homepage where you can choose between Wikipedia, Wikibooks, Wiktionary, etc.

---

## 🛠️ Troubleshooting

### ❌ ZIM file doesn’t show in browser?
- Confirm the port is open: `sudo netstat -tulnp | grep 8080`
- Confirm file path is correct and file is readable

### ❌ SCP fails with "Could not resolve hostname c:"
- You must run SCP **from Windows**, not from the Pi
- Wrap Windows paths in quotes: `"C:\Users\..."`

### ❌ Transfer looks stuck
- Use `rsync -ah --progress` to resume or check progress
- Run `ls -lh` to compare source and destination sizes

### ❌ Browser says "connection refused"
- Make sure Kiwix is running and Pi IP is correct
- Try another port (8081, 8082) if conflicts exist

---

## 🔁 Auto Start Kiwix on Boot (Optional)

### 1. Create a systemd service:
```bash
sudo nano /etc/systemd/system/kiwix.service
```

Paste this:
```ini
[Unit]
Description=Kiwix Offline Server
After=network.target

[Service]
ExecStart=/usr/bin/kiwix-serve --port=8080 --library /home/ezio/kiwix/library.xml
User=ezio
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

### 2. Enable the service:
```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable kiwix
sudo systemctl start kiwix
```

---

## 🧪 Test Setup

To test:
```bash
curl http://localhost:8080
```
Should return HTML if Kiwix is serving properly.

---

## 📱 Optional: Access from Mobile

1. Connect your phone to same Wi-Fi network
2. Open browser and go to: `http://<pi-ip>:8080`
3. You can also use Kiwix Android/iOS app with a custom library URL

---

## 📦 Recommended ZIM Files

- `wikipedia_en_all_maxi_2024-01.zim` – Full English Wikipedia with images (~90GB)
- `wikibooks_en_all_maxi_2021-03.zim` – Free textbooks and manuals
- `wiktionary_en_all_maxi_2024-05.zim` – Offline English dictionary

All available from: https://library.kiwix.org

---

## ✅ Final Notes

Your cyberdeck is now capable of serving full offline knowledge:
- Wikipedia, Wikibooks, Wiktionary
- Runs entirely without internet
- Accessible from any browser on the LAN

For extended usage: create Wi-Fi hotspot, serve media and knowledge, and store backups on NVMe.

Offline doesn’t mean uninformed — welcome to the age of cyber-resilience! 💾📚🔥


### SERVE KIWIKS FROM NVME


---

### 📘 `README.md` — Kiwix Offline Wikipedia Server on Raspberry Pi Cyberdeck

> Turn your cyberdeck into a self-hosted offline knowledge server with full Wikipedia, FreeCodeCamp, iFixit, and other ZIM resources.

---

## 🧰 Requirements

* Raspberry Pi OS (64-bit Bookworm)
* Mounted NVMe drive at `/mnt/nvme`
* ZIM files inside:
  `/mnt/nvme/WIKIPEDIA AND SOFTWARES/zims/`
* Package: `kiwix-tools`

---

## 🚀 Installation & Setup

### 1. Install Kiwix Tools

```bash
sudo apt update
sudo apt install kiwix-tools -y
```

---

### 2. Generate Library Index (XML)

```bash
mkdir -p ~/kiwix-library
kiwix-manage ~/kiwix-library/library.xml add /mnt/nvme/WIKIPEDIA\ AND\ SOFTWARES/zims/*.zim
```

> This creates an index file needed to serve multiple ZIM files.

---

### 3. Serve the Library

```bash
kiwix-serve --port=8081 --library ~/kiwix-library/library.xml
```

Now you can access the offline knowledge base at:
📡 `http://<your-pi-ip>:8081`

---

## 🔁 Auto Start on Boot (Systemd Service)

### 1. Create a systemd service

```bash
sudo nano /etc/systemd/system/kiwix.service
```

### 2. Paste the following:

```ini
[Unit]
Description=Kiwix Offline Wiki Server
After=network.target

[Service]
ExecStart=/usr/bin/kiwix-serve --port=8081 --library /home/ezio/kiwix-library/library.xml
User=ezio
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

### 3. Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable kiwix
sudo systemctl start kiwix
```

---

## 🛠️ Troubleshooting

### ❌ Error: "Unable to add the ZIM file to the internal library"

**Cause:** You're trying to serve a folder instead of a library XML.
✅ **Fix:** Always use `--library` with an XML file created via `kiwix-manage`.

---

### ❌ Error: "Permission denied"

**Cause:** You're trying to move or modify files in `/mnt/nvme` without `sudo`.
✅ **Fix:** Use `sudo` when modifying or moving files on NVMe.

---

### ❌ Can't access from another device?

* Make sure you're on the same network.
* Firewall isn't blocking port 8081.
* Use the Pi's actual IP address, e.g. `http://192.168.254.152:8081`.

---

## ✅ Success!

You now have:

* ✅ Full offline Wikipedia + docs
* ✅ Auto-start on boot
* ✅ Local web server at `http://<pi-ip>:8081`
* ✅ Stored on fast NVMe

