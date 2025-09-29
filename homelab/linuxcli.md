# Linux CLI
### Navigate directory
**pwd** show current directory\
**cd** change directory\
**cd ..** go back a folder\
**mkdir** make folder\
**rmdir** remove folder\
**ls** list contents\
**ls -a** also show hidden\
**ls -la** more detail, shows permissions

### Create / manage files
**echo "contents" > file.txt** create file.txt that includes contents, will overwtite if existing\
**echo "contents" >> file.txt** will add to existing file\
**touch file.txt** creates empty file.txt\
**nano file.txt** edit document in cli or create\
**mousepad file.txt** edit document in gui\
**cp file.txt Music** copy file.txt to Music folder\
**rm file.txt** Delete file\
**mv file.txt Music** move file to Music folder\
**locate file.txt** locate a file, update if not found\
**sudo updatedb** update db for search\
**chmod +rwx file.txt** Change permissions, read write execute\
**chmod 777 file.txt** give read write execute to all

### Users
**sudo adduser john** add user named john\
**su john** switch user to john\
**passwd** change password\
**cat /etc/passwd** read etc/passwd file, shows user ids and services\
**sudo cat /etc/sudoers** check user priveleges\
**grep 'sudo' /etc/group** search specific term in file, sudo in this case

### Networking commands
**ip a** ip info\
**ip n** list ip and associated mac addresses\
**ip r** routing info\
**route** routing table\
**ping 192.168.0.1** ping that address

### Start / autostart services (webserver)
**sudo service apache2 start** starts webserver (go to ip add)\
**sudo service apache2 stop** stops webserver\
**python3 -m http.server 80** starts webserver with python\
**sudo systemctl enable ssh** autostart ssh\
**sudo systemctl disable ssh** disable autostart ssh

### Update
**sudo apt update && apt upgrade** Kali\
**sudo dnf upgrade --refresh** Fedora
