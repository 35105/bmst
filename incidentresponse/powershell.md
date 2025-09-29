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
