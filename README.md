<video src="diagrams_and_wiring/Doomsday-cyberdeck.mp4" controls width="600">
...
📥 [Download Video](diagrams_and_wiring/Doomsday-cyberdeck.mp4)


🛡️ Doomsday Cyberdeck
The Doomsday Cyberdeck is a self-reliant, offline survival computer designed for grid-down scenarios, natural disasters, and world-changing events — inspired by the post-collapse worlds of The Division and The Last of Us. Housed in a Pelican Vault V200 hard case, this portable rig serves as a lifeline when the internet disappears, power is scarce, and information becomes your most valuable asset.

This cyberdeck doesn't just compute — it empowers. It gives you access to:

Offline encyclopedias (like Kiwix/Wikipedia)

Survival guides, DIY manuals, and first aid PDFs

Coding tools, Linux documentation, and offline dev environments

Music, books, videos, and even retro games

And now, an offline AI assistant running a powerful LLM (DeepSeek 32B or higher), ready to help with coding, troubleshooting, or survival strategies.

But it’s not just about storage — it’s about resilience.

Thanks to the Geekworm X1202 UPS, the device runs independently of the power grid:

Charged via standard electrical outlets when available

Or powered by a solar panel in the field when mobility is key

Whether you're setting up a field network, accessing a full technical library, or running AI locally to interpret and assist, this cyberdeck is your bunker in a box.
---
📦 Project Structure

DOOMSDAY-CYBERDECK-GUIDE/
├── 3d_models/           # STL and Fusion 360 files for panels and mounts
├── diagrams_and_wiring/              # Wiring diagrams (PDF, Fritzing, images)
├── hardware_components/              # Build progress, schematics, renderings
├── documnets/                # Manuals, references, offline data info
|-- scripts/
├── README.md

---
Built from Scratch
Every inch of this project was designed and fabricated from the ground up:

✏️ Started with paper sketches

🧩 Modeled using Fusion 360

🧱 Sliced in Ultimaker Cura

🖨️ Printed using a Creality 3D Max

The entire build process — including wiring diagrams and 3D models — is open-source and documented in this repository. If you can print it, you can build it.

🧠 Coding + Setup: Raspberry Pi OS, shell scripting, Kiwix, offline content tools

---
💡 Features
📺 10.1” Waveshare Capacitive Touchscreen (1280×800) — IPS, HDMI input, control buttons.

🧠 Raspberry Pi 5 (16GB RAM) — The brain of the cyberdeck.

💾 256GB MicroSD + 2TB Samsung NVMe via Geekworm PCIe — Massive storage for offline content.

🔋 Geekworm X1202 UPS HAT — Reliable power with dual input (USB-C + barrel jack).

🎛️ Control Panel with:

Momentary push button for Raspberry Pi 5 Power

Momentary push button for Geekworm UPS Power

Toggle switch for screen backlight power

USB-C and barrel jack dual charging

SD Card extender for quick swaps

HDMI extender, MIC, Ethernet splitter (3 ports)

🎮 Bluetooth Mini Keyboard — Wireless, backlit.

🔊 Built-in speakers for media playback.

📚 Offline Library:

Full Wikipedia (Kiwix)

Survival PDFs, Linux books, offline dev docs

Music, movies, and games

DIY electronics, robotics, and IT labs

---

 Wiring
All GPIO and power control circuits are included in the /diagrams_and_wiring/ folder:

Pi power button via GPIO + shutdown script

UPS power toggle using momentary switch

LED toggle wiring via 3-pin switch

USB-C power input for UPS

Fan or temp sensor options (future)

---
🔋 Power System
Geekworm X1202 UPS supports:

USB-C input (standard wall charging)

5.5mm barrel jack input (solar panel compatible)

Can be powered and charged:

From a standard wall outlet

From a portable solar panel when off-grid

Supports hot swap battery backup with 4x 18650 cells

---

🧠 AI Integration
Offline AI Chatbot powered by a local LLM such as:

DeepSeek 32B

or other optimized models for ARM or GPU acceleration

Use cases:

Tech support, survival Q&A, Linux help

Natural language access to offline documentation

Educational chatbot for coding, math, or electronics

Frameworks:

Llama.cpp, GPTQ, ExLlama, or DeepSpeed (based on hardware support)

Potential for voice input/output in future

---
📸 Gallery
📂 images/ folder includes:

Assembled cyberdeck photos

Fusion 360 screenshots

Wiring diagrams and internal views

---

 Offline Content Tips
Check docs/offline-resources.md for:

How to download & install Kiwix (Wikipedia)

DIY manuals, Linux docs, and free books

Self-hosted media servers (e.g., Jellyfin)

Fully offline Linux distros

---

🌐 Offline Content Tips
Check docs/offline-resources.md for:

How to download & install Kiwix (Wikipedia)

DIY manuals, Linux docs, and free books

Self-hosted media servers (e.g., Jellyfin)

Fully offline Linux distros

---
This is more than a Raspberry Pi project. It's a mission-critical companion when the digital world collapses — an archive, an assistant, a media center, a lab, and a survival guide rolled into one.


🧠 Philosophy
"When things hit the fan, I want a laptop that’s more than a laptop — I want a library, lab, and lifeline."

The Doomsday Cyberdeck is built for resilience, information freedom, and digital independence.

