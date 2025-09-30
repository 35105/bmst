# SMB Share

[Home](https://github.com/35105/bmst/tree/main)\
\
[Back](https://github.com/35105/bmst/tree/main/homelab)

Update:
```bash
apt update && apt upgrade -y
```

Create user:
```bash
adduser meatloaf
adduser meatloaf sudo
```

Switch:
```bash
su - meatloaf
```

Give user ownership of mounts:
```bash
sudo chown -R meatloaf:meatloaf /data
sudo chown -R meatloaf:meatloaf /docker
```

Install samba:
```bash
sudo apt install samba
```

Backup config file:
```bash
cd /etc/samba
sudo mv smb.conf smb.conf.old
```

Edit new config:
```bash
sudo nano smb.conf
```

Config:
```
[global]
   server string = Storage
   workgroup = WORKGROUP
   security = user
   map to guest = Bad User
   name resolve order = bcast host
   hosts allow = 10.0.0.0/24
   hosts deny = 0.0.0.0/0
[data]
   path = /data
   force user = meatloaf
   force group = meatloaf
   create mask = 0774
   force create mode = 0774
   directory mask = 0775
   force directory mode = 0775
   browseable = yes
   writable = yes
   read only = no
   guest ok = no
```

Add samba user:
```bash
sudo smbpasswd -a [username]
```

Autostart:
```bash
sudo systemctl enable smbd
sudo systemctl enable nmbd
sudo systemctl restart smbd
sudo systemctl restart nmbd
```

wsdd is required for windows discovery:
```bash
sudo apt install wsdd
sudo apt install wsdd-server
```

Allow on firewall if theres issues:
```bash
sudo ufw allow OpenSSH
sudo ufw allow Samba
# following 3 are needed for wsdd
sudo ufw allow 3702/udp
sudo ufw allow 5357/tcp
sudo ufw allow 5358/tcp
# Check ufw status
sudo ufw status
```

Enable fw, optional:
```bash
sudo ufw enable
```
