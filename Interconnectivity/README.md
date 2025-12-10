# Interconnectivity

This readme documents how I connect to my homelab pc from other devices. It must be noted that for this and most other connections, like for a Minecraft server or code server I use tailscale as it is free, offers low latency and is easy to use.

## SSH

I like to be able to ssh into my server, to do this i use openssh paired with tailscale

### SSH on Fedora (and probably other linux distros)
#### 1. Enable SSH on Fedora

On your Fedora machine, run:
```bash
sudo dnf install openssh-server sudo systemctl enable --now sshd
```

Check it’s running:
```bash
systemctl status sshd
```
You should see **active (running)**.

---

#### 2. Get your Fedora PC’s IP address

Run:
```bash
ip a
```
Look for:
- `wlp…` = WiFi
- `enp…` = Ethernet

Your IP will look like:
`192.168.x.x`
Example: `192.168.1.37`

#### 3. SSH with Tailscale
First ensure both devices have Tailscale installed.
On Fedora:
```bash
tailscale ip -4
```

You’ll see something like: `100.x.x.x`
