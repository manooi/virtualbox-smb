# VM SMB
This repository provides a guide to spinning up a virtual machine (VM) in VirtualBox and configuring it to run an SMB (Samba) server. The SMB server allows file sharing between systems over a network. Follow the steps to set up VirtualBox Guest Additions, configure shared folders, and run an SMB server on your VM.

## Features:
- Install and configure VirtualBox Guest Additions for better VM integration.
- Set up a shared folder between the host and VM using VirtualBox.
- Install and configure an SMB server for network file sharing from the VM.

## Instructions:

### 1. Install Guest Additions
- Download `VBoxGuestAdditions_7.1.0.iso`

- In the bottom right corner, mount the CD.

- Run script
```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
cd /media/<USERNAME>/VBox_GAs_*
sudo ./VBoxLinuxAdditions.run
```

### 2. Share folder in VirtualBox
- In VirtualBox Shared Folder, add new folder.
- Select 'Auto-mount' and 'Make Permanent'
- Add user to `vboxsf` group
  ```bash
  sudo usermod -aG vboxsf <USERNAME>
  # or
  sudo adduser <USERNAME> vboxsf
  ```

### 3. SMB
```bash
# Install smb
sudo apt install smb

# Edit config file
sudo vim /etc/samba/smb.conf

# Add this line
[mysmb]
path = /media/<VIRUTALBOX_SHARED_FOLDER>
writeable = yes
browseable = yes
public=no

# Add smb user
sudo smbpasswd -a <USERNAME> # existing user
sudo systemctl restart smbd

# Check ip address
hostname -I
```


