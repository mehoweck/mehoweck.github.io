# mehoweck's know-how page


## PROXMOX - steps after installation 
based on: https://www.youtube.com/watch?v=2foI-hP1omA

### Remove 'no subscription' popup after logging into PROXMOX

```bash
cd  /usr/share/javascript/proxmox-widget-toolkit/ 
cp proxmoxlib.js proxmoxlib.js.bak
nano proxmoxlib.js
# search for Ext.Msg.show
make the code unactive - comment block of code or negate condition
service pveproxy reload
```

### Remove the enterprise repo 
This repo is needed when you have the subscription. Edit following file and comment line with repo address.
```bash
vim /etc/apt/sources.list.d/pve-enterprise.list
```

### Enable additional update repo
(only for non production installations)

```bash
vim /etc/apt/sources.list
deb http://ftp.debian.org/debian bullseye main contrib
deb http://ftp.debian.org/debian bullseye-updates main contrib

# PVE pve-no-subscription repository provided by proxmox.com,
# not recommended for production uses
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription

# security updates
deb http://security.debian.org/debian-sec... bullseye-security main contrib
```
more about proxmox repositories:
https://pve.proxmox.com/wiki/Package_Repositories

### remove lvm-data partion
Data Center = Storage = LOCAL LVM = delete
lvremove /dev/pve/data
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root

### Refresh the conteners list
pveam update
