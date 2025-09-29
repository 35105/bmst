# pfsense

ZFS Mirror:<br>
In setup choose Install pfSense with ZFS for a mirrored setup.
Select the option to create a ZFS mirror, both drives can be selected<br>
If one drive fails it will use the other<br>
Add S.M.A.R.T status widget to Dashboard and save<br>

Turn off DNS Server Override:<br>
This allows ISP DNS to override and is checked by default<br>
System > General Setup > Uncheck DNS Server Override<br>

To add VLANS:<br>
Interfaces > Assignments > VLANs<br>
Add > Give description and tag, parent interface should be LAN interface<br>
Click interface assignments and add them<br>

DHCP Server:<br>
Set Kea DHCP in System > Advanced > Networking > Server Backend<br>
Will need to set up for each VLAN and LAN where DHCP required<br>
Services > DHCP Server<br>
Select desired interface<br>
Check Enable, 2nd setting<br>
Enter Address pool range<br>
Add static mappings down the bottom, get MAC addresses from Diagnostics > Arp Table<br>

Alias:<br>
Aliases make setting rules easier, you can group IPs, Ports or URLs under a name and then refer to the alias in the rule<br>
Firewall > Aliases > Add<br>
Select Ports/ URLs/ IPs and give a name<br>
Firewall > Rules<br>
Destination ports wont show unless you specify TCP/UDP in protocol<br>

pfBlockerNG:<br>
DNS blocking<br>
System > Package Manager > Available Packages > pfBlockerNG-devel > Install<br>
Firewall > pfBLockerNG<br>
In General set Download Failure Threshold to 2, will not be noisy in logs if unavailable<br>
Feeds is where you add more lists<br>
Once added change state to ON then go to DNSBL Groups and make sure Action is set to Unbound<br>

Backup Config:<br>
Diagnostics > Backup and Restore<br>
Download Configuration as XML<br>
You can restore from the same location using that file<br>
