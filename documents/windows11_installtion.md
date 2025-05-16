
# 🪟 Windows 11 Installation on Raspberry Pi 5 using WoR Flasher

This README provides a complete, step-by-step guide to installing **Windows 11 ARM** on a **Raspberry Pi 5** using **WoR Flasher** via Raspberry Pi OS, all done **headlessly over SSH**.

---

## 🧰 Requirements

- Raspberry Pi 5
- One **microSD card with Raspberry Pi OS** (boot and control system)
- One **microSD card or USB drive (≥64GB)** for Windows installation
- UEFI Firmware microSD card (or EEPROM flashed)
- Internet connection
- Optional: SSH access from another machine

---

## 🔧 Step-by-Step Instructions

### 1. Prepare Raspberry Pi OS (microSD 1)

- Flash Raspberry Pi OS (Lite or Desktop) onto an SD card
- Boot the Pi
- Enable SSH (`sudo raspi-config → Interfacing Options → SSH → Enable`)
- Connect the Pi to your network

### 2. SSH into the Pi

```bash
ssh pi@<raspberry_pi_ip>
```

If you get a host key warning:

```bash
ssh-keygen -R <raspberry_pi_ip>
```

---

### 3. Install WoR Flasher

```bash
sudo apt update
sudo apt install git -y
git clone https://github.com/Botspot/wor-flasher
cd wor-flasher
chmod +x ./install-wor.sh
./install-wor.sh
```

---

### 4. Insert Target USB or SD Card

Insert your **empty USB stick or microSD via USB adapter**. Then check with:

```bash
lsblk
```

**DO NOT SELECT** `/dev/mmcblk0` (that’s your Pi OS!)

---

### 5. Follow Menu Prompts in WoR Flasher

- **Language**: `en-us`
- **Model**: `1` (Raspberry Pi 5)
- **Target**: Select the correct `/dev/sdX` (your USB/SD)
- **Installation Mode**: Choose `1` - Full install to the device
- Wait while the system:
  - Downloads Windows image
  - Flashes it to the drive
  - Prepares drivers

---

## 💾 After Flashing is Done

1. Shut down the Pi:

```bash
sudo shutdown now
```

2. Remove the Pi OS SD card
3. Insert the **UEFI microSD card**
4. Plug in the **Windows-prepped USB/SD**
5. Boot the Pi

In UEFI boot menu:

- Go to `Boot Manager`
- Choose the USB/SD drive
- Windows Setup should begin (first boot may take a few minutes)

---

## 🛠 Troubleshooting

| Issue | Solution |
|-------|----------|
| USB install fails in WoR | Use `diskpart` to clean & convert drive to GPT |
| Windows setup doesn’t start | Check UEFI is installed and booting the correct drive |
| Wrong drive selected | Always double-check with `lsblk`, avoid `/dev/mmcblk0` |
| Host key changed on SSH | Run `ssh-keygen -R <pi_ip>` to fix |
| Partition size issues | Use a 64GB+ USB or SD card, formatted with FAT32/NTFS |

---

## 🔄 Optional: Create UEFI Firmware microSD

1. Download latest release from [RPi4 UEFI GitHub](https://github.com/pftf/RPi4/releases)
2. Format microSD as **FAT32**
3. Extract and copy:
   - `RPI_EFI.fd`
   - `bootaa64.efi`
   - `config.txt`
   - `/EFI` folder
4. Insert into Pi SD slot for UEFI boot

---

## 🧠 Notes

- Boot times can be slow (~2–5 mins first time)
- Performance improves if you use an **NVMe SSD** via USB 3.0 adapter
- To skip UEFI SD in the future, you can flash UEFI to EEPROM

---

## 👑 You’re Done!

You now have Windows 11 ARM running on your Raspberry Pi 5. Enjoy testing, experimenting, or building cool projects with it!
