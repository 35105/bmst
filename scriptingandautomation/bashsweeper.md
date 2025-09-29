# Bash IP Sweeper

```
./ipsweep.sh 192.168.1 > ips.txt
#will run bash script below and place ips into ips.txt file
```

```
#!/bin/bash

if [ "$1" == ""]
then
echo "You forgot an IP address!"
echo "Syntax: ./ipsweep.sh 192.168.1"

for ip in `seq 1 254`; do
ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" &
done
fi

```

```
for ip in $(cat ips.txt); do nmap $ip & done
#will scan the addresses in the ips.txt file
```
