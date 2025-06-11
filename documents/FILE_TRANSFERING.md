

````markdown
# 🎬 Raspberry Pi Media File Transfer & Splitting Guide

This guide documents how to reliably transfer large video files from a Windows PC to a Raspberry Pi over the local network using `scp`, and how to split, upload, extract, and clean up files.

---

## 🔁 1. Problem

Transferring large files (3–5GB) via `scp` often fails due to:
- SSH connection resets
- Weak Wi-Fi or power supply on Raspberry Pi
- Memory limits or USB I/O issues

---

## 🧰 2. Solution: Split Large Files into Chunks

Use `7-Zip` to split large files into 500MB chunks before transferring.

### ✅ Step 1: Split the file on Windows using PowerShell

```powershell
& "C:\Program Files\7-Zip\7z.exe" a -v500m "C:\Path\To\Output\filename-split.7z" "C:\Path\To\Original\filename.mkv"
````

This will create:

```
filename-split.7z.001
filename-split.7z.002
...
filename-split.7z.00X
```

---

## 📤 3. Transfer to Raspberry Pi using `scp`

In Git Bash or PowerShell:

```bash
scp filename-split.7z.* ezio@<PI_IP>:/home/ezio/
```

* Replace `<PI_IP>` with the actual IP of your Pi.
* If a file fails mid-transfer, just re-send that specific part.

Example:

```bash
scp filename-split.7z.005 ezio@<rasperry pi ip>:/home/ezio/
```

---

## 📦 4. Extract on the Raspberry Pi

SSH into the Raspberry Pi:

```bash
ssh ezio@<rasperry pi ip>
```

Then extract the archive:

```bash
7z x filename-split.7z.001
```

> This will automatically stitch all parts and restore the full `.mkv` file.

---

## 🧹 5. Clean Up

Once confirmed that the video file is extracted and working:

```bash
rm filename-split.7z.*
```

This will delete all split parts to free up space.

---

## ✅ Optional: Batch Split All Episodes

To split all `.mkv` files in a folder at once:

```powershell
Get-ChildItem "C:\Path\To\Videos\*.mkv" | ForEach-Object {
    $file = $_.FullName
    $base = $_.BaseName -replace '\.mkv$', ''
    & "C:\Program Files\7-Zip\7z.exe" a -v500m "$($_.Directory)\$base-split.7z" $file
}
```

---

## 🧠 Notes

* Ensure `7-Zip` is installed: [https://www.7-zip.org/](https://www.7-zip.org/)
* Git Bash or PowerShell can both be used for `scp`.
* Recommended to use a stable network and power supply for Raspberry Pi.

---

📁 *Created by: Ezio for local media preservation and off-grid viewing on Raspberry Pi cyberdeck.*

```

---

Would you like me to save this as a downloadable `.md` file for you now?
```
