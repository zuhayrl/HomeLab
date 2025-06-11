# HomeLab

## Setting up WSL

**WSL - Windows Subsystem for Linux is a built in linux terminal for windows.**

Run the command prompt or powershell in administrator mode. Then type in the following command:
``` 
wsl --install
```
This will by default install Ubuntu. If you wish to choose another distro then run `wsl --list --online` to see a list of available distros and run `wsl --install -d <DistroName>` to install a distro.


## Setting Up a Samba Server on your Pi

1. **First update your Pi:**
  ```
  sudo apt-get update
  sudo apt-get upgrade
  ```

2. **Then install the samba package:**
  ```
  sudo apt-get install samba samba-common-bin
  ```

3. **Now you need to choose the folder you would like to share:**
  - Create a new folder:
  ```
  mkdir /home/username/shared
  ```
  - Choose an existing folder:
  (I like to set my entire drive as the folder so that i can easily access any files from my main pc)

4. **Next we need to configure the samba config file `smb.conf` to handle access:**
  ```
  [pishare]
  path = /home/username/shared
  writeable = yes
  browseable = yes
  create mask=0777directory mask=0777
  public=no
  ```
  
  `[pishare]` : The name of the share. This is what shows up on the network when browsing shared folders.
  
  `path = /home/username/shared` : The actual location of the shared folder on the Raspberry Pi.
  
  `writeable = yes` : Allows users to create, modify, or delete files within the shared folder.
  
  `browseable = yes` : Makes the share visible when browsing the network.
  
  `create mask = 0777` : Sets full permissions (read, write, execute) for all newly created files in the shared folder.
  
  `directory mask = 0777` : Sets full permissions for newly created directories.
  
  `public = no` : Disables guest access — only authenticated users can access the share.

5. **Set a username and password:**
	Type your username in place of `<username>`
  ```
  sudo smbpasswd -a <username>
  ```

  You will then be prompted to enter a password:
  
  ```
  New SMB password:
  Retype new SMB password:
  ```

  You will then see the following if done correctly:
 
  ```
  Added user USERNAME
  ```

6. **Restart the samba server:**
  ```
  sudo systemctl restart smbd
  ```
  If we need to get the hostname, we can obtain it with `hostname -I` .

7. Connect to the server on your device:
  Go to File Explorer > Computer > Map network drive
  Input drive location/name: `\\hostname\sharename`
  For example using my local IP address and the share name from step 4 (the text between the [] ): `\\192.168.1.100\pishare`
  
  
