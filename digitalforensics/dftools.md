# Digital Forensics Tools

[Home](https://github.com/35105/bmst/tree/main)\
\
[Back](https://github.com/35105/bmst/tree/main/digitalforensics)

## Disk Analysis - Autopsy

New Case
add data source - Disk Image or VM file
Ingest modules are automated actions to save time
Bottom right corner you will see a progress bar that will let you know when each ingest module has been completed
Artifact tree on left will populate as it analyses	 
Data sources in artifact tree will show drives, partition table

**Ingest Modules:**  
Each module analyses and extracts specific data from the drive. Set up Autopsy to run modules during the source-adding phase by right-clicking the data source, selecting modules, and clicking finish.

**File Types:**  
Be aware that adversaries may rename files with misleading extensions, leading to misclassification by extension but correct classification by MIME type.

**Data Sources Summary:**  
This summary provides an overview in nine categories. For detailed findings, analyse each module separately using the “Result Viewer”.

**Create Report:**  
The report contains all information from the “Result Viewer” but lacks search options, requiring manual searches for specific events. Use “Generate Report” to create it. After selecting the format and scope, Autopsy will generate the report. Click on the “HTML Report” section to view it in your browser, which includes all results from the “Result Viewer” pane.

Autopsy can strain low-resource systems, causing freezes with long results. To avoid this, generate a report to analyse data without needing Autopsy.

## Disk Imager: FTK Imager

**Snapshot Ram**
File > Capture Memory
Can use volatility to analyse

**Hard Drive Imaging**
File > Create Disk Image.
Drive type, physical
Select drive
Evidence item info (chain of custody)
output destination
set fragment size to 0mb, disk image wont be split into small segments

## Hashing and Integrity

**Windows** 
get-filehash < file > - SHA256 hash
get-filehash -algorithm md5 < file > - md5 or sha1

**Linux**
sha256sum < file >
md5sum < file >
sha1sum < file >
echo -n < text > | string - string of text EG echo -n Collingwood 2023! | md5sum

## KAPE

In a digital investigation, a disk imager should be initiated to create a complete disk image of the target system, while KAPE runs concurrently to quickly gather crucial evidence for rapid results.

KAPE can be deployed on a large scale using PowerShell, allowing for the download, execution, and return of results to the security team.

Top Left: The Targets section allows us to specify exactly what information we want to extract from the target system, enabling swift data retrieval. This can include anything from system memory to web browser data.
Top Right: The Modules section enhances functionality, enabling operations on the retrieved data, such as analyzing information collected from the target system. These modules build on the Target options, allowing us to refine the data we wish to collect.
Bottom: The Command-Line section constructs the query that will be executed by KAPE.

To enable Targets in the top left, we first need to specify the target source, typically a disk image.

Next, set a location for saving the files collected by KAPE.

Select our targets; gather information from popular web browsers like Chrome, Edge, and Firefox. Scroll through the Targets box to find them and execute the command.

KAPE will launch a terminal and begin retrieving copies of the requested files.

## Volatility

**Volatility 2**

List all processes that were running. List active and closed network connections. View internet history (Internet Explorer). Identify files on the system and retrieve them from the memory dump. Read the contents of Notepad documents. Retrieve commands entered into the Windows Command Prompt (CMD). Scan for the presence of malware using YARA rules. Retrieve screenshots and clipboard contents. Retrieve hashed passwords. Retrieve SSL keys and certificates.

**Command List**

volatility -f memdump.mem imageinfo analyzes memory image “memdump.mem” to determine the suggested profile for analysis which includes the operating system version and architecture.

volatility -f memdump.mem --profile=PROFILE pslist provides the profile and uses the pslist plugin to print a list of processes to the terminal.

volatility -f memdump.mem --profile=PROFILE pstree uses the pstree plugin to print a process tree to the terminal.

volatility -f memdump.mem --profile=PROFILE psscan uses the psscan plugin to print all available processes including hidden ones often used by malware compare this to pslist for differences.

volatility -f memdump.mem --profile=PROFILE psxview uses the psxview plugin to print expected and hidden processes combining pslist and psscan.

volatility -f memdump.mem --profile=PROFILE procdump -p PIDHERE uses the procdump plugin to save a process as a file requiring a process ID with the -p flag.

volatility -f memdump.mem --profile=PROFILE netscan uses the netscan plugin to identify active or closed network connections.

volatility -f memdump.mem --profile=PROFILE timeliner uses the timeliner plugin to create a timeline of events from the memory image.

volatility -f memdump.mem --profile=PROFILE iehistory uses the iehistory plugin to pull internet browsing history.

volatility -f memdump.mem --profile=PROFILE filescan uses the filescan plugin to identify files on the system from the memory image.

volatility -f memdump.mem --profile=PROFILE cmdline -p PIDHERE uses the cmdline plugin to retrieve command-line arguments for a process requiring a process ID with the -p flag.

volatility -f memdump.mem --profile=PROFILE dumpfiles -n --dump-dir=./ uses the dumpfiles plugin to retrieve files from the memory image saving them in the current directory.

**Additional Resources**

Download open-source memory dumps from Volatility’s GitHub link specifically created for analysis with Volatility. A comprehensive list of useful Volatility commands can be found here.

**Analyze Dumps**

cd /volatility/

python vol.py -f /home/ubuntu/Desktop/Volatility Exercise/memdump1.mem imageinfo to get profile first in suggested profiles (Win7SP1x64)

python vol.py -f /home/ubuntu/Desktop/Volatility Exercise/memdump1.mem --profile=Win7SP1x64 pslist | grep “svchost.exe” process list

python vol.py -f /home/ubuntu/Desktop/Volatility Exercise/memdump2.mem --profile=Win7SP1x64 procdump -p 2940 --dump-dir /home/ubuntu/Desktop/Volatility Exercise/ dump specific process.

Volatility 3

The --profile option is no longer present in Volatility 3. Generic plugin names have been replaced with OS-specific plugins. Use pstree for Windows Linux and Mac. Cheatsheet: Volatility Cheatsheet.

Volatility Workbench

Only compatible with Windows. Browse Image Refresh Process List Select OS and command In the right corner you can copy or export results.

## Memory, Pagefile, and Hibernation File

**Pagefile.sys**
The Pagefile.sys is used within Windows operating systems to store data from the RAM when it becomes full. Through normal use, RAM is filled by the system and then Windows is able to identify which data to move from it to the Pagefile.sys where it can remain until required again. It can also be used as a backup of data in the event of a system crash.

**Swap file in Linux**
Linux uses swap space to store RAM when it is full or when the data is not in current use. Within Linux however, traditionally it is a swap partition rather than a swap file and is therefore separate from the other files as it is contained on its own partition.
sudo fallocate -l [file size] /swapfile - create swapfile
free -h command - total, used and free swap space on the system.
swapon –show - identify whether the swap space is a file or a partition. 

**Hibernation File**
Starting with Windows 2000, Microsoft introduced the hibernation feature that allows the operating system to store the current state of operation when you turn off the computer, or the system goes into sleep mode. During hibernation everything from memory is copied to the disk in a file called hiberfil.sys, when the computer is restored, the system moves to the saved state. Hibernation files are a good source of information for digital forensic practitioners, as they store data in RAM file without having to run special tools.

## Metadata and File Carving

**Exiftool**
Kali Linux - exiftool
sudo apt-get install exiftool
exiftool < filename >

exiftool.org to dl for Windows
sudo apt-get install libimage-exiftool-perl for Ubuntu
sudo dnf install exiftool for Debian

Get Metadata:
exiftool filename.jpg
exiftool -TagName filename.jpg - specify tag
exiftool -TagsFromFile source.jpg target.jpg - copy metadata

**Scalpel**
Linux command-line tool scalpel
use the command man scalpel to see what it can do
When attempting to use scalpel on a disk image file (.img) the tool will communicate with it’s configuration file, which is located at /etc/scalpel/scalpel.conf by default. This configuration file allows us to define what file types we’re trying to search for.
nano config or open it in gui
locate desired files and remove #
scalpel -o < output > < disk image file > EG: scalpel -o /root/Desktop/ScalpelOutput DiskImage1.img
File will be in folder on desktop

**chown dommand**
change owner of file/ folder linux
chown [options] new_owner[:new_group] file(s) (chown new_owner file) EG: chown ubuntu q3.conf
to check ownership ls -l q3.conf

## Powershell / procdump

take the output from Get-Process, and search for the string 'calc' to only show the information we want
Get-Process | findstr -I calc
.procdump.exe -ma 1688 (number just to the left of program on very right side)
I had to use .\procdump.exe

