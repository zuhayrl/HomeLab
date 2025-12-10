# **Tailscale + Fabric Minecraft Server Tutorial**

## Why use Tailscale
Allows you to create a server without port forwarding, or exposing your computer.

---

## 1. Install Tailscale

### Windows 
1. Go to **[https://tailscale.com](https://tailscale.com)**
2. Download and install **Tailscale**
3. Log in (Google / GitHub works)
4. Leave it running

Your PC now has a **Tailscale IP** (private, usually `100.x.x.x`)

### Linux (Fedora)

**Install:**
```bash
sudo dnf install tailscale
```

**Enable and start Tailscale as a service:**
```bash
sudo systemctl enable --now tailscaled
```

**Authenticate your device:**
```bash
sudo tailscale up
```

It will give you a login link → open it in your browser → sign into your Tailscale account → approve the machine.

Once done, you’re connected.

## 2. Server Setup
### Find server IP
Get the server's Tailscale IP address
```bash
tailscale ip
```

The MC port is by default 25565

Your server will have the address: `<Tailscale ip>:25565`

### 3. Users
To connect to the server, the user must download Tailscale and create an account. Then the admin will need to approve them in the Tailscale admin panel. Then they use the server address.

### 4. MC Server Settings
**Enable Whitelist**
In MC console:
```minecraft
/whitelist on
/whitelist add <playername>
```
Verify server properties:
```minecraft
online-mode=true
white-list=true
enable-rcon=false
```

## 5. Custom Server IP

If you’re running a **Minecraft server over Tailscale**, you _don’t_ have to use the raw Tailscale IP (100.x.x.x).  
You can instead give your machine a **custom DNS name** inside Tailscale so players (or just you) can connect using:
```
myserver.tailnet-name.ts.net
```
Or even shorter if MagicDNS is on:
```
myserver
```

**Here’s how to rename it:**
### Option 1 — Rename from the machine itself

Run:
```
sudo tailscale set --hostname=myserver
```
Replace **myserver** with any name you want.

Then restart Tailscale:
```
sudo systemctl restart tailscaled
```
Done. Your device will appear as **myserver** in the Tailscale admin panel.

### Option 2 — Rename from the Tailscale Admin Panel

1. Go to: [https://login.tailscale.com](https://login.tailscale.com)
2. Open **Machines**
3. Find your Fedora device
4. Click the **⋮ menu (3 dots)** → **Rename machine**
5. Enter your new name, e.g. **mc-host**

### Connect to the Minecraft server using the new name

If MagicDNS is **enabled** (you can check under DNS settings in the admin panel):

You can connect from another Tailscale device with:
```
mc-host
```
or:
```
mc-host.tailnet-name.ts.net
```

Minecraft will resolve it automatically.
If MagicDNS is **off**, use:
`100.x.x.x:25565`
(But it’s better to enable MagicDNS.)

## Useful Commands
|Action|Command|
|---|---|
|Check status|`tailscale status`|
|See your Tailscale IP|`tailscale ip -4`|
|Disconnect|`sudo tailscale down`|
|Reconnect|`sudo tailscale up`|
|Restart service|`sudo systemctl restart tailscaled`|
