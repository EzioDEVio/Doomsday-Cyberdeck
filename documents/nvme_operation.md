
# 🧠 Ezio's Doomsday Cyberdeck Vault - README

> This README serves as a complete step-by-step guide for setting up an offline, multimedia-capable, web-accessible vault hosted on a Raspberry Pi 5 with a Geekworm UPS HAT (X1202) and a 2TB NVMe drive. This vault includes videos, music, books, survival manuals, PDFs, programming tools, and more. It functions without internet access and features a custom web interface with previews and search functionality.

---

## 🔧 1. Detecting and Mounting the NVMe Drive

### 1.1 Plug in the NVMe
Ensure your NVMe drive is properly seated in the Geekworm HAT and the PCIe FFC cable is connected to the correct Pi port.

### 1.2 Detect the Drive
```bash
lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL
```
Look for `nvme0n1` and its partitions (e.g., `nvme0n1p1`, `nvme0n1p2`).

### 1.3 Mount the Drive (Read-Only)
```bash
sudo mkdir -p /mnt/nvme
sudo mount -t exfat -o ro,uid=33,gid=33 /dev/nvme0n1p2 /mnt/nvme
```

### 1.4 Mount the Drive (Read-Write, for Editing)
```bash
sudo umount /mnt/nvme
sudo mount -t exfat -o rw,uid=33,gid=33 /dev/nvme0n1p2 /mnt/nvme
```

### 1.5 Add to `/etc/fstab` for Persistent Mount (Optional)
```bash
sudo nano /etc/fstab
```
Add:
```
/dev/nvme0n1p2 /mnt/nvme exfat ro,uid=33,gid=33,nofail 0 0
```
Use `rw` instead of `ro` if you want permanent write access.

---

## 🌐 2. Setting Up the Offline Web Server

### 2.1 Install Lighttpd
```bash
sudo apt update
sudo apt install lighttpd -y
```

### 2.2 Set Document Root to NVMe
```bash
sudo nano /etc/lighttpd/lighttpd.conf
```
Change:
```
server.document-root = "/var/www/html"
```
To:
```
server.document-root = "/mnt/nvme"
```

### 2.3 Enable Directory Listing
```bash
sudo lighttpd-enable-mod dir-listing
sudo systemctl restart lighttpd
```

### 2.4 Configure MIME Types
Ensure `/etc/mime.types` includes:
```
video/mp4               mp4
audio/mpeg              mp3
application/pdf         pdf
```
Then restart:
```bash
sudo systemctl restart lighttpd
```

---

## 🧠 3. Creating the `index.html` Dashboard

### 3.1 Create/Edit the File
```bash
sudo nano /mnt/nvme/index.html
```

### 3.2 Include These Features:
- Responsive Grid of Cards
- Folder Links
- Modal Previews (MP4, MP3, PDF)
- Search Functionality

### 3.3 Mount as RW → Edit → Mount as RO
```bash
sudo umount /mnt/nvme
sudo mount -t exfat -o rw,uid=33,gid=33 /dev/nvme0n1p2 /mnt/nvme
# Edit and Save index.html
sudo umount /mnt/nvme
sudo mount -t exfat -o ro,uid=33,gid=33 /dev/nvme0n1p2 /mnt/nvme
```

---

## 💡 4. Using the Web Interface

### Features Implemented:
- 🔍 Search: Live filtering by folder name
- ▶ Preview: Click any media type to open it in a modal
- 📁 Clickable Folders: Open in new browser tabs

### Supported Formats:
- `*.mp4` (H.264 recommended)
- `*.mp3`
- `*.pdf`

To access:
```
http://<your-pi-ip>
```
Use your browser on any device connected to the Pi's network.

---


---

## 🛠️ 6. Troubleshooting

| Issue                     | Fix                                                                 |
|--------------------------|----------------------------------------------------------------------|
| 403 Forbidden Error      | Mount with `uid=33,gid=33` and enable `dir-listing` in Lighttpd       |
| Video doesn’t play       | Confirm it's `H.264` codec, not HEVC; check MIME types                |
| PDF not loading          | Ensure correct path and check browser console                        |
| Search not working       | Make sure folder names are stored as `data-folder` attributes        |

---

## 🧪 7. Next Steps / Future Features
- Auto-scan real files and subfolders
- Auto-generate thumbnails using ffmpeg
- Add login-based access (optional)
- Integrate offline chatbot (GPT-based)

---

## 📦 Directory Layout Example
```
/mnt/nvme
├── index.html
├── BOOKS/
│   └── sample.pdf
├── FULL MOVIES/
│   └── sample.mp4
├── MUSIC/
│   └── sample.mp3
├── SURVIVAL MATERIALS/
│   └── field-guide.pdf
├── SOFTWARES/
│   └── python3.deb
```

---

Built by Ezio + AI Copilot 👨‍🚀📁💥
> Resilient. Offline. Prepared.

