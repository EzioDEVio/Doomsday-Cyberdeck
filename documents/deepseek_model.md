

This guide outlines how to run **DeepSeek / LLaMA models** locally on your Raspberry Pi 5 using `Ollama` and the `Open WebUI` interface.

---

## ✅ Requirements

* Raspberry Pi 5 with Raspberry Pi OS
* Ollama installed (`ollama serve`)
* Docker & Docker Compose installed
* Open WebUI cloned or pulled with `docker-compose.yml`

---

## 🚀 Steps to Start Ollama + Open WebUI

### 1. Start Ollama (host)

```bash
ollama serve
```

This starts the Ollama backend server on port `11434`.

### 2. Navigate to Open WebUI folder

```bash
cd ~/open-webui
```

### 3. Restart Open WebUI container

```bash
docker-compose down
docker-compose up -d
```

---

## 🛠 Docker Compose Configuration

Make sure the environment in `docker-compose.yml` points to the host Ollama backend correctly:

```yaml
services:
  open-webui:
    image: ghcr.io/open-webui/open-webui:latest
    container_name: open-webui
    ports:
      - "3000:8080"
    environment:
      - OLLAMA_BASE_URL=http://host.docker.internal:11434
      - DISABLE_EMBEDDINGS=true
    volumes:
      - ./webui_data:/app/backend/data
```

> ⚠️ `host.docker.internal` lets the Docker container access the host's `Ollama` running on `127.0.0.1`.

---

## 🌐 Access Open WebUI

In your browser:

```bash
http://127.0.0.1:3000
```

If running locally on the Pi with desktop UI:

```bash
http://localhost:3000
```

---

## ✅ Result

You should see Open WebUI load up and list your models (e.g., `deepseek-r1:1.5b`, `llama2`). You can now chat with them through the browser.

---

## 💡 Notes

* Ensure `ollama` is running *before* `docker-compose up`
* If you ever reboot: repeat the steps:

  1. `ollama serve`
  2. `cd ~/open-webui && docker-compose up -d`

---

Enjoy your local, offline-ready AI assistant! 🤖💻






## RUNNING WEB-UI ON THE BOOT








---

### 🔧 Optional: Make it start automatically at boot (Recommended for cyberdeck use)

#### 1. **Create a systemd service for Ollama:**

Create a file:

```bash
sudo nano /etc/systemd/system/ollama.service
```

Paste:

```ini
[Unit]
Description=Ollama Server
After=network.target

[Service]
ExecStart=/usr/local/bin/ollama serve
Restart=always
User=ezio
Environment=OLLAMA_KEEP_ALIVE=1

[Install]
WantedBy=multi-user.target
```

> Make sure `/usr/local/bin/ollama` is the correct path (`which ollama` to confirm).

Then:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable ollama
sudo systemctl start ollama
```

---

#### 2. **Auto-start WebUI via Docker:**

Docker will auto-restart Open WebUI if you update `docker-compose.yml` like this:

```yaml
    restart: always
```

Full example:

```yaml
services:
  open-webui:
    image: ghcr.io/open-webui/open-webui:latest
    container_name: open-webui
    ports:
      - "3000:8080"
    environment:
      - OLLAMA_BASE_URL=http://127.0.0.1:11434
      - DISABLE_EMBEDDINGS=true
    volumes:
      - ./webui_data:/app/backend/data
    restart: always  # <--- Add this line
```

Then:

```bash
cd ~/open-webui
docker-compose down
docker-compose up -d
```

---

### 🔁 From now on, after a reboot:

* Ollama will start automatically.
* Open WebUI will come up via Docker.
* Access it at: [http://127.0.0.1:3000](http://127.0.0.1:3000) or your Pi’s IP.

---
























sudo apt install curl -y
curl -fsSL https://ollama.com/install.sh | sh
ollama run deepseek-r1:1.5B


Full installation link:

https://medium.com/@TechHutTV/run-deepseek-r1-on-a-raspberry-pi-5-e58902a6dbbc

my ip to access  the ollama UI

IP<rasperry pi>:4000

