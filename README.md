# HomeLab

## Setting up WSL

**WSL - Windows Subsystem for Linux is a built in linux terminal for windows.**

Run the command prompt or powershell in administrator mode. Then type in the following command:
``` 
wsl --install
```
This will by default install Ubuntu. If you wish to choose another distro then run `wsl --list --online` to see a list of available distros and run `wsl --install -d <DistroName>` to install a distro.


## Setting Up a Samba Server on your Pi

First update your Pi:
```
sudo apt-get update
sudo apt-get upgrade
```

Then install the samba package:
```
sudo apt-get install samba samba-common-bin
```

Now you need to choose the folder you would like to share:
1. Create a new folder:
```
mkdir /home/username/shared
```
2. Choose an existing folder:
(I like to set my entire drive as the folder so that i can easily access any files from my main pc)

Next we need to configure the samba config file `smb.conf` to handle access:
