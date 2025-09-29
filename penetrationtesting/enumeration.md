# Enumeration

### Dir mode brute forces web directories

gobuster dir -u "http://www.example.com/" -w /usr/share/wordlists/dirb/small.txt -t 64

**gobuster dir** directory and file enumeration mode
**-u** "http://www.example.com/" target
**-w /usr/share/wordlists/dirb/small.txt** directs Gobuster to use the small.txt wordlist to brute force the web directories with GET requests
**-t 64** sets the number of threads Gobuster will use to 64, improves performance.

gobuster dir -u "http://www.example.com" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js

### Dns mode brute forces subdomains.

gobuster dns -d example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt

**gobuster dns** enumerates subdomains on the configured domain.
**-d example.com** target
**-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt** sets the wordlist  Gobuster uses each entry of this list to construct a new DNS query. If the first entry of this list is 'all', the query would be all.example.thm.

### Vhost mode brute forces virtual hosts

gobuster vhost -u "http://example.com" -w /path/to/wordlist

gobuster vhost -u "http://10.201.56.63" --domain example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320

**gobuster** numerate virtual hosts.
**-u "http://10.201.56.63"** sets the URL to browse to 10.201.56.63.
**-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt** configures Gobuster to use the subdomains-top1million-5000.txt wordlist. Gobuster appends each entry in the wordlist to the configured domain. If no domain is explicitly configured with the --domain flag, Gobuster will extract it from the URL. If any subdomains are found, Gobuster will report them to you in the terminal.
**--domain example.thm** sets the top- and second-level domains in the Hostname: part of the request to example.thm.
**--append-domain** appends the configured domain to each entry in the wordlist. If this flag is not configured, the set hostname would be www, blog, etc. This will cause the command to work incorrectly and display false positives.
**--exclude-length** filters the responses we get from the sent web requests. With this flag, we can filter out the false positives. If you run the command without this flag, you will notice you will get a lot of false positives like "Found: Orion.example.thm Status: 404 [Size: 279]" or  "Found: pm.example.thm Status: 404 [Size: 276]". These false positives typically have a similar response size, so we can use this to filter out most false positives. We expect to get a 200 OK response back to have a true positive.
