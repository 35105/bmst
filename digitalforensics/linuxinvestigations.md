# Linux Investigations

[Home](https://github.com/35105/bmst/tree/main)\
\
[Back](https://github.com/35105/bmst/tree/main/digitalforensics)

## passwd/shadow

/etc/passwd - Used to keep track of every registered user that has access to a system\
/etc/shadow - Contains encrypted passwords as well as other information such as account or password expiration values. The /etc/shadow file is readable only by the root account.

## /Var/Lib and /Var/Log

var/lib - in Debian /var/lib/dpkg/status shows a list of installed software, copy to desktop and open in text editor
	
cat status | grep Package > packages.txt - can use this to copy package names to a text file

cat status will read the file.\
grep Package will search for any lines containing ‘Package’\
	> packages.txt will output the results to a text file called packages.txt
	
## var/log - os logs
	
/var/log/auth.log – Contains system authentication information, including user logins.\
/var/log/dpkg.log – Contains information that is logged when a package is installed or removed using the ‘dpkg’ command.\
/var/log/btmp – This file contains information about failed login attempts.\
/var/log/cron – Whenever the cron daemon starts a cron job, it logs the information about the cron job in this file.\
/var/log/secure – Contains information related to authentication and authorization privileges.\
/var/log/faillog – Contains user failed login attempts.
		
## web server logs
	
var/log/apache2/access.'log
			
The client IP address making the request\
The resource they are trying to access\
The HTTP method, which will most often be GET (to get a resource, such as images in a web page)\
The user-agent used by the client IP (typically browser etc) and the timestamp of the request\
			
EG: 52.50.100.106 - SBTUser [27/Jul/2020:15:30:00 -0600] "GET /logo.png HTTP/1.1" 200 379
		
52.50.100.106 - The IP address of the client making the request to the web server\
SBTUser - The userid of the account making the request\
[27/Jul/2020:15:30:00 -0600] - The timestamp of the request\
“GET /logo.png HTTP/1.1” - The resource that is being accessed\
200 - The HTTP response code. In this case it is 200 OK (successfully accessed). Could be 404.\
379 - The size of the file sent to the client (in this case, the size of logo.png)

## User Files

ls -a - show hidden\
cat .bash_history better than history in terminal, history can be erased with history -c\
terminal where the commands were entered needs to be closed before the commands can be written to the history file

## Clear Files

These are any files that are accessible through standard means, such as the terminal or the graphical browser.\
	
## Steganography

On our Desktop we have a text file secretmessage.txt, the same text file but inside a ZIP container secretmessage.zip, and an image of a dog Dog.jpg.\
Using the command cat Dog.jpg secretmessage.zip > Dog2.jpg we can insert the ZIP file into the image of a dog, creating a new file named Dog2.jpg\
It can still be opened like a normal image file. But, if we use the unzip command on the image file, we’ll retrieve the files that were inside the hidden ZIP file.
	
steghide embed -cf Dog.jpg -ef secretmessage.\

steghide – summons the tool we want to use\
embed – selects the operation we want to use, in this case embedding a file in another\
-cf Dog.jpg – the ‘cover file’ flag is where we state the file that will hold the hidden file\
-ef secretmessage – the ’embed file’ flag is where we state the file we want to hide inside the cover file
		
steghide extract -sf Dog.jpg.

steghide – summons the tool\
extract – selects the extract operation\
-sf Dog.jpg – the ‘steganography file’ flag is used to tell steghide which file we believe contains data hidden via steganography, which is our cover file from above.
		
## Hiding strings in metadata
	
exiftool -Comment="Super Sneaky!" Dog.jpg\
Becomes comment in meetadata
		
## Memory

LiME (https://github.com/504ensicsLabs/LiME) and memdump (https://manpages.ubuntu.com/manpages/trusty/man1/memdump.1.html).

