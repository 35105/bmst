# bmst
Notes I made for myself which may or may not be useful.

## Table of Contents

- [Homelab](#homelab)
  - [Backup](#backup)
  - [Jellyfin](#jellyfin)
  - [pfsense](#pfsense)
  - [Proxmox](#proxmox)
  - [SMB Share](#smb-share)
  - [WOW Server](#wow-server)
- [Python](#python)
  - [Subnet Calculator](#subnet-calculator)
- [Template Documents](#template-documents)
  - [Incident Response Plan](#incident-response-plan)
  - [Phishing Report](#phishing-report)
- [Tools](#tools)
  - [Autopsy](#autopsy)
  - [Burp Suite](#burp-suite)
  - [CMD](#cmd)
  - [DeepBlueCLI](#deepbluecli)
  - [ExifTool](#exiftool)
  - [FTK Imager](#ftk-imager)
  - [Nmap](#nmap)
  - [Powershell](#powershell)
  - [Scalpel](#scalpel)
  - [Snort](#snort)

## Homelab

### Backup
[Top of Page](#bmst)

Proxmox Backup Server


### Jellyfin
[Top of Page](#bmst)


### pfsense
[Top of Page](#bmst)

ZFS Mirror:<br>
In setup choose Install pfSense with ZFS for a mirrored setup<br>
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

### Proxmox
[Top of Page](#bmst)

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

### SMB Share
[Top of Page](#bmst)

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

### WOW Server
[Top of Page](#bmst)


## Python

### Subnet Calculator
[Top of Page](#bmst)

## Template Documents

### Incident Response Plan
[Top of Page](#bmst)

Preparation - Incident Response Plan

**Preparation**
	
Develop response plans<br>
Ensure resources needed by response team are ready for use<br>
Train and evaluate performance to ensure team members are capable of completing duties<br>
	
**Identification**

Guidance on how to report, what information to gather.
	
When?<br>
Who discovered it?<br>
How?<br>
What was affected?<br>
Does its affect operations?<br>
What is the scope?<br>
				
Criticality level: How fast does response need to be?<br>
Impact level: How long will it impact the business?<br>
			
**Containment**

What actions should be taken?
		
Disconnect devices?<br>
Power off systems?<br>
		
How will this effect collection of evidence?
	
Would powering off lose volatile evidence eg memory?
		
Document guidelines and procedures for evidence acquisition
	
Restore from backups so systems can resume without infected machines
	
**Eradication**

What happened? Analyse
	
Work backward from MITRE ATT&CK<br>
Review SIEM logs
		
Remove malicious artifacts
	
Malware?<br>
System changes?<br>
Remove persistance?
		
Harden systems
	
Apply patches<br>
Empower automated defense with indicators of compromise found<br>
Create runbooks
		
**Recovery**

Move affected systems back into production
	
Scan etc, ensure they're no longer infected
		
**Lessons Learned**

Hold a meeting including stakeholders
	
Recap what happened<br>
What worked well?<br>
How could response improve?
		
Should drive change, eg rewriting documentation, more budget for tools etc

### Phishing Report
[Top of Page](#bmst)

Email Description: Brief description of email. What is it trying to do? 2 or 3 sentences.

Open email as a text file to get email based artifacts below:

Email Sender (search "From")

Subject Line (search "Subject") 

Date (search "Date")

Recipient (search "To") 

Reply-To (search "reply")

Sending Server IP (search X-Sender-IP")

To get Reverse DNS, go to https://whois.domaintools.com and enter the Sending Server IP #####

Reverse DNS (copy "Resolve Host" from whois search)

URL (Right click link in e-mail and "copy hyperlink", can also search for http or <a> in text file)

URL Analysis

Enter domain into https://www.virustotal.com/gui/home/upload to check reputation<br>
https://www.wannabrowser.net/ & https://www.url2png.com to get more info<br>
https://urlscan.io/ will be a bit more in depth than url2png<br>

How was information found? :

Virustotal results brief sentence analysis and URL

WannaBrowser and URL2PNG results brief sentence analysis and URL

Defensive Measures

Assessment, how old is the domain? Was is created for this purpose? (how old is it?) Should entire domain be blocked or just a specific URL? Depends on if someone will need access, was it hijacked? (Older) If so block the specific URL.

Block Web Artifacts: Briefly explain recommended actions based on the above.

Block Email Email Artifacts: Briefly explain recommended actions. If the domain is used purely for malicious activity block the whole domain, if its a common email like gmail or outlook then block the specific email address in the gateway.

## Tools

### Autopsy
[Top of Page](#bmst)

Create new case:<br>
Base Directory: The main directory designated to hold all files related to the case<br>
Case Type: Local (Single-user) or hosted on a server for multiple analysts to access (Multi-user).<br>
Case files have a “.aut” file extension<br>

Ingest modules:<br>
Each Ingest Module is created to analyze and extract particular data from the drive. You can set up Autopsy to execute specific modules during the source-adding phase. Access the “Run Ingest Modules” menu by right-clicking on the data source. Select the modules you wish to apply and then click the finish button.

File Types:<br>
Pay attention to File Types. An adversary may rename a file with a misleading extension, causing it to be miscategorized by extension but correctly categorized by MIME type.

Data Sources Summary:<br>
The Data Sources Summary offers a brief overview in nine categories. For detailed findings and specific artifacts, analyze each module separately using the “Result Viewer”.

Create Report:<br>
The report includes all information from the “Result Viewer” pane but lacks additional search options, requiring manual artifact searches for specific events. Use the “Generate Report” option to create reports. After selecting your report format and scope, Autopsy will generate the report. Click on the “HTML Report” section to view it in your browser, where it will include all results from the “Result Viewer” pane on the left side.

The Autopsy tool can strain low-resource systems, causing freezes when browsing long results. To avoid this, use reports: parse the data and generate a report, then analyse it without needing Autopsy.

### CMD
[Top of Page](#bmst)

attrib                  : Displays or changes file attributes.<br>
cipher                  : Manages encryption on NTFS volumes.<br>
cipher /w:              : Wipes free space on a drive to prevent file recovery.<br>
dir                     : Lists files and directories for navigation.<br>
fsutil                  : Manages file systems and hard drive properties.<br>
getmac                  : Displays MAC addresses of network interfaces.<br>
net localgroup          : Manages local groups, adding or removing users.<br>
net user                : Manages user accounts on the local machine.<br>
netsh                   : Configures and manages network settings and firewall rules.<br>
netsh wlan show profiles: Displays saved Wi-Fi profiles and security settings.<br>
netstat                 : Displays active network connections and listening ports.<br>
netstat -ano            : Displays active connections with associated process IDs.<br>
robocopy                : Advanced file copying with mirroring and logging options.<br>
route                   : Displays and modifies the IP routing table.<br>
schtasks                : Schedules and manages tasks on local or remote computers.<br>
sc query | more         : List all services and detailed information about each one.<br>
taskkill                : Terminates a running process by PID or name.<br>
tasklist                : Lists currently running processes, including PIDs.<br>
tasklist /svc           : Lists running processes with associated services.<br>
tracert                 : Traces the route packets take to reach a destination.<br>
wmic process get description, executablepath : display running processes and the associated binary file that was executed to create the process.<br>

### DeepBlueCLI
[Top of Page](#bmst)

Used for digital forensics and incident response. Analyse various data sources and extract relevant artefacts. It facilitates reporting, query execution, and automation of tasks, enhancing the efficiency of forensic investigations.

admin level PowerShell

Cd to folder, launch program and log with :  .\DeepBlue.ps1 ..\Log1.evtx
The .. takes you back folders, to go forward in folders to file just add the folder names.\folder1\insidefolder1\Log1.evtx

Windows will block it : Set-ExecutionPolicy Bypass -Scope CurrentUser to fix.

Run again : ./DeepBlue.ps1 ../Log1.evtx

DeepBlue can access the local system's Security or System event logs directly : ./DeepBlue.ps1 -log security , ./DeepBlue.ps1 -log system

to export to a text file : ./DeepBlue.ps1 .\evtx\* > output.txt

Examples:<br>
Event log manipulation                    .\DeepBlue.ps1 .\evtx\disablestop-eventlog.evtx<br>
Metasploit native target (security)       .\DeepBlue.ps1 .\evtx\metasploit-psexec-native-target-security.evtx<br>
Metasploit native target (system)         .\DeepBlue.ps1 .\evtx\metasploit-psexec-native-target-system.evtx<br>
Metasploit PowerShell target (security)   .\DeepBlue.ps1 .\evtx\metasploit-psexec-powershell-target-security.evtx<br>
Metasploit PowerShell target (system)     .\DeepBlue.ps1 .\evtx\metasploit-psexec-powershell-target-system.evtx<br>
Mimikatz lsadump::sam                     .\DeepBlue.ps1 .\evtx\mimikatz-privesc-hashdump.evtx<br>
New user creation                         .\DeepBlue.ps1 .\evtx\new-user-security.evtx<br>
Obfuscation (encoding)                    .\DeepBlue.ps1 .\evtx\Powershell-Invoke-Obfuscation-encoding-menu.evtx<br>
Obfuscation (string)                      .\DeepBlue.ps1 .\evtx\Powershell-Invoke-Obfuscation-string-menu.evtx<br>
Password guessing                         .\DeepBlue.ps1 .\evtx\smb-password-guessing-security.evtx<br>
Password spraying                         .\DeepBlue.ps1 .\evtx\password-spray.evtx<br>
PowerSploit (security)                    .\DeepBlue.ps1 .\evtx\powersploit-security.evtx<br>
PowerSploit (system)                      .\DeepBlue.ps1 .\evtx\powersploit-system.evtx<br>
PSAttack                                  .\DeepBlue.ps1 .\evtx\psattack-security.evtx<br>
User added to administrator group         .\DeepBlue.ps1 .\evtx\new-user-security.evtx<br>


Outputs:<br>
CSV                   .\DeepBlue.ps1 .\evtx\psattack-security.evtx | ConvertTo-Csv<br>
Format list (default) .\DeepBlue.ps1 .\evtx\psattack-security.evtx | Format-List<br>
Format table          .\DeepBlue.ps1 .\evtx\psattack-security.evtx | Format-Table<br>
GridView              .\DeepBlue.ps1 .\evtx\psattack-security.evtx | Out-GridView<br>
HTML                  .\DeepBlue.ps1 .\evtx\psattack-security.evtx | ConvertTo-Html<br>
JSON                  .\DeepBlue.ps1 .\evtx\psattack-security.evtx | ConvertTo-Json<br>
XML                   .\DeepBlue.ps1 .\evtx\psattack-security.evtx | ConvertTo-Xml<br>

### ExifTool
[Top of Page](#bmst)
Read/Write/Edit Metadata

Download:<br>
exiftool.org to dl for Windows<br>
sudo apt-get install libimage-exiftool-perl for Ubuntu<br>
sudo dnf install exiftool for Debian<br>

Get Metadata:<br>
exiftool filename.jpg<br>
exiftool -TagName filename.jpg - specify tag<br>
exiftool -TagsFromFile source.jpg target.jpg - copy metadata<br>

### FTK Imager
[Top of Page](#bmst)
Create and analyse disk images in digital forensics

Snapshot Ram:<br>
File > Capture Memory<br>
Can use volatility to analyse<br>

Hard Drive Imaging:<br>
File > Create Disk Image.<br>
Drive type, physical<br>
Select drive<br>
Evidence item info (chain of custody)<br>
Output destination<br>
Set fragment size to 0mb, disk image wont be split into small segments<br>

### Nmap
[Top of Page](#bmst)

nmap -sS target                     Performs a TCP SYN scan to identify open ports stealthily.<br>
nmap -sT target                     Conducts a TCP connect scan to find open ports by establishing full connections.<br>
nmap -sU target                     Scans for open UDP ports on the target.<br>
nmap -p port target                 Scans specific port(s) or a range (e.g., -p 22,80).<br>
nmap -A target                      Enables OS detection, version detection, and script scanning for detailed information.<br>
nmap -O target                      Attempts to determine the operating system of the target.<br>
nmap -sV target                     Detects service versions running on open ports.<br>
nmap -Pn target                     Skips host discovery, treating all specified hosts as online, useful for firewalled hosts.<br>
nmap -T4 target                     Sets the timing template to speed up the scan (T4 is faster but may be more detectable).<br>
nmap -oN filename target            Outputs scan results to a file in normal format.<br>
nmap -oX filename target            Outputs scan results to a file in XML format.<br>
nmap --script script target         Runs a specific Nmap script against the target for additional checks (e.g., --script http-enum).<br>
nmap -sP target                     Performs a ping scan to discover live hosts on the network.<br>
nmap -6 target                      Scans for IPv6 addresses.<br>

### Powershell
[Top of Page](#bmst)

Get-NetIPConfiguration and Get-NetIPAddress                 : get network-related information<br>
Get-LocalUser                                               : list all users on system<br>
Get-LocalUser -Name BTLO | select *                         : provide a specific user to the command to only get information about them<br>
Get-Service | Where Status -eq "Running" | Out-GridView     : quickly identify running services on the system, show in grid view<br>
Get-Process | Format-Table -View priority                   : group running processes by their priority value<br>
Get-Process -Id 'idhere' | Select *                         : specific information (-Name ‘namehere’) or the Id. * provides all the properties<br>
Get-ScheduledTask                                           : list tasks that are set to run after certain conditions are met. Scheduled Tasks used for persistence.<br>
Get-ScheduledTask -TaskName 'PutANameHere' | Select *       : dig deeper by specifying the task we’re interested in, and retrieving all properties for it.<br>
tasklist > tasklist.txt                                     : put tasklist into a text file<br>
Get-EventLog -LogName Security                              : retrieve security event logs for auditing and analysis<br>
Get-EventLog -LogName Application                           : retrieve application event logs for troubleshooting<br>
Get-WinEvent -LogName 'Microsoft-Windows-Security-Auditing' : access detailed security auditing logs<br>
Get-FileHash -Path 'filepath'                               : compute the hash value of a file for integrity verification<br>
Get-Content 'filepath'                                      : read the contents of a file, useful for examining logs<br>
Get-ChildItem -Path 'C:\Path\To\Directory' -Recurse         : list all files and directories recursively, useful for file system analysis<br>
Get-Command | Where-Object { $_.Source -like '*module*' }   : list all available modules and their commands<br>
Get-Process | Where-Object { $_.CPU -gt 100 }               : identify processes consuming high CPU resources<br>

### Scalpel
[Top of Page](#bmst)

Linux command-line tool scalpel<br>
use the command man scalpel to see what it can do<br>
When attempting to use scalpel on a disk image file (.img) the tool will communicate with it’s configuration file, which is located at /etc/scalpel/scalpel.conf by default. This configuration file allows us to define what file types we’re trying to search for.<br>
nano config or open it in gui<br>
locate desired files and remove #<br>
scalpel -o output disk image file EG: scalpel -o /root/Desktop/ScalpelOutput DiskImage1.img<br>
File will be in folder on desktop<br>

### Snort
[Top of Page](#bmst)

Common Examples:<br>
alert	tcp	any	any	->	192.168.1.0/24	80	(msg:"HTTP Traffic Detected";)<br>
log  udp	 any	 any 	->	10.0.0.0/8	53	(msg:"DNS Query Logged";)<br>
pass	 icmp	any	any	->	192.168.1.0/24	any	(msg:"Allow ICMP Traffic";)<br>
drop tcp	 any	 any 	->	192.168.1.0/24	22	(msg:"Block SSH Access";)<br>

alert tcp any any -> 192.168.1.0/24 80 (msg:"HTTP Traffic Detected"; sid:1000001; rev:1;)<br>
This rule generates an alert for any TCP traffic directed to the 192.168.1.0/24 subnet on port 80.<br>

log udp any any -> 10.0.0.0/8 53 (msg:"DNS Query Logged"; sid:1000002; rev:1;)<br>
This rule logs any UDP traffic directed to the 10.0.0.0/8 subnet on port 53, which is used for DNS queries.<br>

pass icmp any any -> 192.168.1.0/24 any (msg:"Allow ICMP Traffic"; sid:1000003; rev:1;)<br>
This rule allows all ICMP traffic to the 192.168.1.0/24 subnet, effectively permitting ping requests and other ICMP messages.<br>

drop tcp any any -> 192.168.1.0/24 22 (msg:"Block SSH Access"; sid:1000004; rev:1;)<br>
This rule drops any TCP traffic directed to the 192.168.1.0/24 subnet on port 22, blocking SSH access to that subnet.<br>
