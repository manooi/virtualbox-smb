# VM SMB
Spin up vm in virtualbox, and use it to run smb server.


## 1. Install Guest Additions
- Download `VBoxGuestAdditions_7.1.0.iso`

- In the bottom right corner, mount the CD.

- Run script
```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
cd /media/<USERNAME>/VBox_GAs_*
sudo ./VBoxLinuxAdditions.run
```

## 2. Share folder in VirtualBox
- In VirtualBox Shared Folder, add new folder.
- Select 'Auto-mount' and 'Make Permanent'
- Add user to `vboxsf` group
  ```bash
  sudo usermod -aG vboxsf <USERNAME>
  # or
  sudo adduser <USERNAME> vboxsf
  ```

## 3. SMB
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


