Below is a **clear, beginner-friendly Tailscale tutorial specifically for a Fabric Minecraft server**.
This works on **Windows or Linux**, needs **no port forwarding**, and is **free + safe**.

---

# 🧷 **Tailscale + Fabric Minecraft Server Tutorial**

## What you’ll end up with

* A Fabric server running on your PC
* Friends connecting through **Tailscale** like it’s LAN
* **No public IP exposure**
* **No router configuration**

---

## ✅ Requirements

* A PC to host the server (Windows or Linux)
* Java 17+ (for modern Minecraft)
* A Fabric server already installed (or we can do it now)
* All players install **Tailscale**

---

## 🧩 Step 1 — Install Tailscale (Host PC)

1. Go to **[https://tailscale.com](https://tailscale.com)**
2. Download and install **Tailscale**
3. Log in (Google / GitHub works)
4. Leave it running

✅ Your PC now has a **Tailscale IP** (private, usually `100.x.x.x`)

---

### 🔍 Find your Tailscale IP

* Open Tailscale app → **“This device”**
* Or via terminal:

  ```
  tailscale ip
  ```

Example:

```
100.85.14.23
```

---

## 🧩 Step 2 — Run Your Fabric Server (Host PC)

Start your server normally:

```bash
java -Xms2G -Xmx4G -jar fabric-server-launch.jar nogui
```

✅ Minecraft listens on:

```
0.0.0.0:25565
```

(This is correct — don’t change it.)

---

## 🧩 Step 3 — Install Tailscale (Friends)

Each friend:

1. Installs **Tailscale**
2. Logs in (can be different accounts)
3. You approve them in the admin panel (first time only)

✅ Once connected, they can see your device

---

## 🧩 Step 4 — Friends Join the Server

In Minecraft → **Multiplayer → Direct Connect**

Server Address:

```
<YOUR_TAILSCALE_IP>:25565
```

Example:

```
100.85.14.23:25565
```

✅ That’s it — they are playing.

---

## 🔐 Step 5 — Lock Down Security (IMPORTANT)

### Enable whitelist

In server console:

```
/whitelist on
/whitelist add <playername>
```

---

### Verify `server.properties`

Check these:

```
online-mode=true
white-list=true
enable-rcon=false
```

✅ Prevents spoofing
✅ Prevents random connections

---

## 🧠 Optional: Make It Even Cleaner (Recommended)

### Rename server IP

Tell friends:

> “This is a **private LAN server via Tailscale**”

They don’t need:

* Hamachi
* Port forwarding
* IP knowledge

---

### Mobile/Multiple devices

You can install Tailscale on:

* Laptop
* Phone hotspot
* Secondary PC

Same server works.

---

## 🛠 Common Problems & Fixes

### ❌ Can’t connect?

* Both sides must be **connected to Tailscale**
* Check **firewall** allows Java
* Try:

  ```
  tailscale ping <host-ip>
  ```

---

### ❌ Fabric server crashes?

* Make sure Java version matches MC version
* Remove client-only mods
* Check logs in `/logs/latest.log`

---

## ⚠️ Limitations (be aware)

* Not for **public servers**
* Works best for **<10 players**
* Host PC must be **online**

---

## ✅ Final Architecture (simple)

```
Friends ──┐
         │ Tailscale private network
Server ──┘
```

No router. No public internet exposure.

---

## ✅ TL;DR (one screen)

1. Install Tailscale on host + friends
2. Run Fabric server
3. Share your **100.x.x.x** IP
4. Whitelist players
5. Play safely

---

If you want:

* 🔁 **Auto-launch on startup**
* 🗺 Dynmap via Tailscale
* 🪟 Windows firewall rules
* 🐧 Linux systemd service
* 🧩 Mod compatibility check (Waystones, JEI, etc.)

Tell me your **OS + Minecraft version**, and I’ll tailor it exactly.

