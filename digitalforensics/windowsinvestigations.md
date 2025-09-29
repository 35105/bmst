# Windows Investigations

## Program artifacts
    
LNK Files: LNK files in Windows OS serve as shortcuts that link one file to another, allowing users to access applications regardless of their location in the file system. They contain valuable metadata, including the linked folder's location, creation and modification dates, last accessed date, and file size. To view these files in a human-readable format, we can use Windows File Analyzer. C:\Users\$USER$\AppData\Roaming\Microsoft\Windows\Recent
    
Windows File Analyzer > Analyze Shortcuts >Recent Items > OK \
All LNK files in one panel\
Double click to recieve more info and save to a text file/ copy to copy and paste
    
Prefetch Files: Prefetch files can provide useful information about programs including the name of the application, the path to the executable file, when the program was last run, and when the program was created/installed. To view these files in a human-readable format, use Prefetch Explorer Command Line also known as PECmd.exe. C:\Windows\Prefetch
    
Administrator Command Prompt > cd Windows >cd Prefetch > dir to show files\
copy to desktop with copy OUTLOOK.EXE-74475F66.pf C:\Users\Whatever\Desktop\
Navigate to where PECmd.exe file is located > PECmd.exe -f C:\Users\Whatever\Desktop\OUTLOOK.EXE-74475F66.pf\
Top of file while show run count, created, modified and last accessed\
Also shows files accessed by this application
    	
PECmd.exe -k “plaguerat.ps1” -d "C:\Users\BTLOTest\Desktop\Windows Investigation One\Prefetch\" -- -k will match to strings
    
Jump List Files: Using the Windows Jump List feature we are able to find two different types of files: automaticDestination-ms and customDestination-ms. These files contain information about applications that are pinned to the taskbar, such as the file path, timestamps, and application identifiers (AppIDs). The Jump List files can be found at:\
C:\Users%USERNAME\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations\
C:\Users\%USERNAME%\AppData\ Roaming\Microsoft\Windows\Recent\CustomDestinations

cmd prompt > cd to address above (customdestinations) and use dir to view, copy to desktop\
jumplist explorer to view\
absolute path will show the name, when lnk created, modified and last accessed

## Logon Events

Event ID 4624 (Successful Logon)\
ID 4672 (Special Logon)\
ID 4625 (Failed Logon)\
and ID 4634 (Logoff)

Windows Event Logs are stored at the following location: C:\Windows\System32\winevt\Logs. The logs we're interested in are stored in the \Security folder of this location.

Analyze using Windows Event Viewer, could also be in SIEM.

4624 Successful Logon

One of the most important properties for us to note is the Logon Type, where there are 8 possible values:

2 – Interactive (interactively logged on, meaning a physical logon to the device)\
3 – Network (accessed system via network)\
4 – Batch (started as an automated batch job)\
5 – Service (a Windows service started by service controller)\
6 – Proxy (proxy logon; not used in Windows NT or Windows 2000)\
7 – Unlock (unlock workstation - think Interactive logon, but unlocking to resume a previous session)\
8 – NetworkCleartext (network logon with cleartext credentials)\
9 – NewCredentials (used by RunAs when the /netonly option is used)
	
4672 Successful Special Logon

Special Logon events are when a user with administrative privileges logs into the system. The Logon ID is a useful randomly-generated value that allows us to keep track of logon sessions, can use ID to find logoff. Logged timestamp is important info.

4625 Failed Logon

ERROR CODE	DESCRIPTION
0xC0000064	The specified user does not exist
0xC000006A	The value provided as the current password is not correct
0xC000006C	Password policy not met
0xC000006D	The attempted logon is invalid due to a bad user name
0xC000006E	User account restriction has prevented successful login
0xC000006F	The user account has time restrictions and may not be logged onto at this time
0xC0000070	The user is restricted and may not log on from the source workstation
0xC0000071	The user account’s password has expired
0xC0000072	The user account is currently disabled
0xC000009A	Insufficient system resources
0xC0000193	The user’s account has expired
0xC0000224	User must change his password before he logs on the first time
0xC0000234	The user account has been automatically locked
	
4634 Logoff

## Recycle Bin

C:\$Recycle.Bin

Tools

Command Prompt (CMD)
RBCmd - GitHub - EricZimmerman/RBCmd: Recycle bin artifact parser
CSVQuickViewer

cmd prompt> cd C:\$Recycle.Bin > dir /a (to show hidden files)

Will show security identifiers, to get usernames: C:\$Recycle.Bin>wmic useraccount get name,SID

Move into the folder associated with his account's SID with tab, dir /a will show hidden.

Files beginning with '$R' conmtain true file contents, '$I' is the metadata for those files.

RBCmd - run this in the bin folder of selected SID/ user:

C:\Users\BTLOTest\Desktop\Recycle-Bin-Analysis\Tools\RBCmd.exe -f $I1UOZ51.xlsx (or necessary file)

-d flag will do directory instead of -f for file:

C:\Users\BTLOTest\Desktop\Recycle-Bin-Analysis\Tools\RBCmd.exe -d . --csv "C:\Users\BTLOTest\Desktop\RBCmdOutput"

-csv outputs to a csv file in specified location

use CSVQuickViewer to easily see when files were deleted and when.

If, for some reason, we wanted to analyze the Recycle Bin for all local accounts on a system, we could run RBCmd from the C:$Recycle.Bin directory, and the use (-d . ) argument to point the tool at this directory. It will then recursively go through each of the SID sub-folders looking for $I files to analyze.

## Browsers

**Atrifacts**

Cookies\
Favorites\
Downloaded Files\
URLs Visited\
Searches\
Cached Webpage\
Cached Images\
	
	
## Tools

**KAPE**

Target Source
Target Destination
Select browser based targets: Edge, Firefox and Chrome
Execute, files will be stored at Target Destination

**Browser History Capturer/Viewer**

BHV can be used on it own but will fail to retrieve Microsoft Edge files if running on the system being investigated.
	
Run Browser History Capturer
Select user profile
Will put inside Capture folder in BHC folder, or select target destination
Capture
	
Open and import to BHV
File > Load History
"Load history captured using the Browser History Capturer tool"
Navigate to capture folder, click load
	
Pane 1 (Main): Website History, Cached Images, and Cached Web Pages
Pane 2 (Bottom): Website Visit Count, showing how many resources have been accessed per month
Pane 3 (Right): Filter Pane, allowing us to filter by keywords
