
# n8n Installation Guide (Docker on Raspberry Pi)

## Table of Contents

1. [Which Method Should You Use?](#which-method-should-you-use)
2. [Prerequisites](#prerequisites)
3. [Method 1: docker run (Quick Start)](#method-1-docker-run-quick-start)
4. [Method 2: docker-compose (Recommended)](#method-2-docker-compose-recommended)
5. [Method 3: systemd Service (Advanced)](#method-3-systemd-service-advanced)
6. [Access n8n Web UI](#access-n8n-web-ui)
7. [Updating n8n](#updating-n8n)
8. [Notes](#notes)


This guide outlines three ways to install and run [n8n](https://n8n.io/) — a powerful workflow automation tool — on a Raspberry Pi using Docker. All methods work on Raspberry Pi models with a **64-bit OS** (e.g. Raspberry Pi OS 64-bit, Ubuntu Server 64-bit).

---

## Which Method Should You Use?

| Method             | Best For                                                                 | Auto Start on Boot | Persistent Config | Easy to Update |
|--------------------|--------------------------------------------------------------------------|--------------------|-------------------|----------------|
| `docker run`       | Quick testing or temporary use                                           | ❌ No               | ✅ Yes (with volume) | ✅ Yes         |
| `docker-compose`   | Persistent deployments, easy updates, and readable config                | ✅ Yes (`restart`)  | ✅ Yes             | ✅ Yes         |
| `systemd service`  | Maximum control over auto-start, logs, and monitoring                    | ✅ Yes              | ✅ Yes             | ✅ Yes         |

---

## Prerequisites

1. Raspberry Pi with a **64-bit OS**
2. Docker installed  
   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   sudo usermod -aG docker $USER
   ```

*(Log out and back in after adding to Docker group)*

3. (Optional but recommended) Docker Compose

   ```bash
   sudo apt install docker-compose
   ```

---

## Method 1: `docker run` (Quick Start)

Run the following command to start n8n interactively:

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

To run in the background (detached mode):

```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

Your workflows will be stored in `~/.n8n`.

---

## Method 2: `docker-compose` (Recommended)

Create a `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - 5678:5678
    environment:
      - TZ=Africa/Johannesburg
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=your_secure_password
    volumes:
      - ~/.n8n:/home/node/.n8n
```
Replace `your_secure_password` with something strong.

Run:

```bash
docker-compose up -d
```

To stop:

```bash
docker-compose down
```

---

## Method 3: `systemd` Service (Advanced)

For full control of auto-start, logs, and crash recovery.

### 1. Create a Systemd Service File

```bash
sudo nano /etc/systemd/system/n8n.service
```

### 2. Paste the Following Content

```ini
[Unit]
Description=n8n workflow automation
After=network.target docker.service
Requires=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker run --rm \
  --name n8n \
  -p 5678:5678 \
  -v /home/pi/.n8n:/home/node/.n8n \
  -e N8N_BASIC_AUTH_ACTIVE=true \
  -e N8N_BASIC_AUTH_USER=admin \
  -e N8N_BASIC_AUTH_PASSWORD=your_secure_password \
  n8nio/n8n
ExecStop=/usr/bin/docker stop n8n

[Install]
WantedBy=multi-user.target
```

Replace `/home/pi/.n8n` if your user directory differs.
Also replace `your_secure_password`.

---

### 3. Enable and Start the Service

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable n8n
sudo systemctl start n8n
```

### 4. Manage the Service

```bash
sudo systemctl status n8n      # View status
sudo systemctl stop n8n        # Stop the service
journalctl -u n8n -f           # View logs live
```

---

## Access n8n Web UI

Go to:
**http\://\<raspberry\_pi\_ip>:5678**

If using auth, login with the credentials set in `N8N_BASIC_AUTH_USER` and `N8N_BASIC_AUTH_PASSWORD`.

---

## Updating n8n

```bash
# Stop service or container
sudo systemctl stop n8n        # or docker-compose down

# Pull latest image
docker pull n8nio/n8n

# Restart
sudo systemctl start n8n       # or docker-compose up -d
```

---

## Notes

* This guide assumes the use of a 64-bit OS. Run `uname -m` — you want to see `aarch64`, not `armv7l`.
* The n8n image supports ARM architecture as of latest versions (source: [DockerHub](https://hub.docker.com/r/n8nio/n8n)).

---
