# **Create a Minecraft Fabric Server**
## Install Fabric Server
### 1. Choose a directory
Choose a directory in which you want to put your MC server. Once in this directory, download the fabric installer and run it here in server mode:
``` bash
java -jar fabric-installer-1.1.0.jar server -dir . -downloadMinecraft
```

Breakdown:
- `server` → server mode
- `-dir .` → install into current folder
- `-downloadMinecraft` → downloads the official server.jar
- If you want a specific version, add: `-mcversion 1.20.1` 
Example with version:

```bash
java -jar fabric-installer-1.1.0.jar server -dir . -mcversion 1.20.1 -downloadMinecraft
```

### 2. Run it for the first time
From inside the server folder:

```bash
java -jar fabric-server-launch.jar
```
It will fail the first time and generate:
- `eula.txt`

### 3. Accept the EULA
Edit the eula.txt and change `eula=false` to `eula=true`. This can be done with `nano eula.txt`.

### 4. Start it for real
```bash
java -Xmx4G -Xms2G -jar fabric-server-launch.jar nogui

```

- `-Xmx4G` - max RAM set to 4GB
- `-Xms2G` - base RAM set to 2GB
This can be adjusted if needed

### 5. Add mods (optional)
Add mods to the `mods` directory

## Create start and stop script

**Create a `start.sh` file with the following contents (adjust RAM as desired).**
```bash
#!/bin/bash
java -Xmx4G -Xms2G -jar fabric-server-launch.jar nogui
```

**Make it executable**
```bash
chmod +x start.sh
```

**Run it**
```bash
./start.sh
```
or
```bash
sh start.sh
```

**If you want it to restart itself after a crash:**
```bash
#!/bin/bash
while true; do
    java -Xmx4G -Xms2G -jar fabric-server-launch.jar nogui
    echo "Server crashed or stopped. Restarting in 5 seconds..."
    sleep 5
done
```

**Create a `stop.sh` script.**
```bash
#!/bin/bash
# Stop Minecraft server gracefully

# Find the PID of the Minecraft server
PID=$(pgrep -f "fabric-server-launch.jar")

if [ -z "$PID" ]; then
    echo "Minecraft server is not running."
    exit 0
fi

# Send SIGINT to gracefully stop the server
kill -2 $PID
echo "Sent stop signal to Minecraft server (PID: $PID)."
```

## Headless start and stop

I prefer to SSH into my server pc and don't like that Minecraft servers use an active terminal. To allow control over SSH without stopping the server when the SSH connection breaks we can run the server in 'headless' mode.

We will use the `screen` package which allows you to detach and reattach to a console/terminal.

### Headless start
Create a `start_headless.sh`:
```bash
#!/bin/bash

screen -dmS minecraft bash -c 'while true; do
    java -Xmx4G -Xms2G -jar fabric-server-launch.jar nogui
    echo "Server crashed or stopped. Restarting in 5 seconds..."
    sleep 5
done'

echo "Minecraft server started in screen session 'minecraft'"
echo "Use 'screen -r minecraft' to attach to the console"

# detach with Ctrl+A and then D
```

**Note:**
- view mc console with `screen -r minecraft`
- detach with Ctrl+A and then D

### Headless stop
Create a `stop_headless.sh`:
```bash
#!/bin/bash
# Stop Minecraft server running in screen session gracefully

# Check if screen session exists
if ! screen -list | grep -q "minecraft"; then
    echo "Minecraft server screen session is not running."
    exit 0
fi

# Find the PID of the Minecraft server Java process
PID=$(pgrep -f "fabric-server-launch.jar" | head -1)
if [ -z "$PID" ]; then
    echo "Minecraft server process not found, killing screen session..."
    screen -S minecraft -X quit
    exit 0
fi

echo "Found Minecraft server (PID: $PID). Sending stop command..."

# Send the 'stop' command to the Minecraft server via screen
screen -S minecraft -X stuff "stop$(printf \\r)"

echo "Stop command sent. Waiting for server to shut down gracefully..."

# Wait for the server to stop (max 30 seconds)
for i in {1..30}; do
    if ! pgrep -f "fabric-server-launch.jar" > /dev/null; then
        echo "Server stopped successfully."
        break
    fi
    sleep 1
done

# Check if server is still running
if pgrep -f "fabric-server-launch.jar" > /dev/null; then
    echo "Server did not stop gracefully. Forcing shutdown..."
    kill -2 $PID
    sleep 5
fi

# Kill the screen session to stop the restart loop
echo "Terminating screen session..."
screen -S minecraft -X quit

echo "Minecraft server stopped and screen session terminated."
```

