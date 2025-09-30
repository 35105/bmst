# Proxmox

[Home](https://github.com/35105/bmst/tree/main)\
\
[Back](https://github.com/35105/bmst/tree/main/homelab)

Disable Enterprise Repos:<br>
Node > Repositories (Under Update) Disable Enterprise Repos<br>
Click Add, select no subscription repository. Click update.<br>

Create ZFS Pools:<br>
Disks > ZFS > Create: ZFS<br>
Can add multiple to pool, make sure they are wiped first<br>
Click box that says add to storage<br>

Create Container in Proxmox:<br>
Local storage > CT Templates and download Ubuntu 22.04<br>
Top right, create CT<br>
Set hostname & password<br>
Give a static IP under network<br>
Confirm install<br>

Add mount point (Where the storage will appear in the containers directory):<br>
Click LXC > Resources > Add > Mount Point<br>
I set it to /data, will appear in data folder in main directory, select size, uncheck backup.<br>
