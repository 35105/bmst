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
