# Logging and Aggregation

Syslog message

Priority Value
		
Priority Value is derived from both the Facility Code and the Severity Level
(facility code * 8) + Severity value = PRI.
			
Header
		
Timestamp, Hostname, Application name, Message ID
			 
Message
		
This could be simple readable text or only machine-readable.


Facility Code	Description
				0	kernel
				1	user-level messages
				2	mail system
				3	system daemons
				4	security/authorization messages
				5	syslogd (internal system messages)
				6	line printer subsystem
				7	network news subsystem
				8	UUCP (Unix-to-Unix Copy Protocol)
				9	clock daemon
				10	security/authorization messages
				11	FTP daemon
				12	NTP (Network Time Protocol)
				13	log audit
				14	log alert
				15	clock daemon

Severity Level	Description
				0	Emergency
				1	Alert
				2	Critical
				3	Error
				4	Warning
				5	Notice
				6	Informational
				7	Debug
				
Windows Event Logs

Location

Windows 2000 to WinXP/Windows Server 2003:
%WinDir%\system32\Config*.evt
Windows Server 2008 to 2019, and Windows Vista to Win10:
%WinDir%\system32\WinEVT\Logs*.evtx
		
Registered events
	
Application: Events logged by an application (Execution, Deployment error, etc.)
		System: Events logged by the Operating System (Device loading, startup errors, etc.)
		Security: Events that are relevant to the security of the system (Logins and logouts, file deletion, granting of administration permissions, etc.)
		Directory Service: This is a record available only to Domain Controllers, it stores Active Directory (AD) events.
		DNS Server: It is a record available only to DNS servers; logs of DNS service are stored.
		File Replication Service: Is a record available only for Domain Controllers, it stores Domain Controller Replication events.
		
Further Reading
	
If you are interested in learning more about these types of records, how they work and how to visualize them, visit the following links:
https://www.manageengine.eu/network-monitoring/Eventlog_Tutorial_Part_I.html
https://www.loggly.com/ultimate-guide/windows-logging-basics/#
		
Security Event Logs
	
Account logon events (valid and invalid sign-ons and sign-offs)
Account management (creation, modification, interaction and deletion of user accounts)
Privilege use.
Resource usage (file creation, modification, interaction and deletion)
		
Further Reading
			
				https://docs.nxlog.co/integrate/windows-security-audit.html
				https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx
				https://www.andreafortuna.org/2019/06/12/windows-security-event-logs-my-own-cheatsheet/
				
Event Viewer - Custom Views
	
Filter what you want
Click custom Views on left
Create Custom View on right
		
				
Sysmon

Considered a better way to use Event Viewer, more detailed logs.	
	
https://youtu.be/9qsP5h033Qk?t=491.
	
Download Sysmon from the Sysinternals website here. Extract, open a command prompt as administrator, move to the location. sysmon -i to begin the install, Agree EULA.
	
To see logs, create custom view in Event Viewer, filter by source - Sysmon, tick all even level options.
	
Can be a lot of noise if added to SIEM, can use config file to limit. An example of a Sysmon configuration file can be found here – https://github.com/SwiftOnSecurity/sysmon-config
	
Other Logs

Microsoft Azure

Azure Monitor - logs from vm, virtual network, azure active directory, azure security centre.
Log Analytic Workspaces
Kusto Query Language (KQL) to query logs
		
Amazon Web Services
	
Amazon uses their own API
If you want to learn more about the AWS API, you can find guides on their documentation subdomain – https://docs.aws.amazon.com/index.html
		
OSQuery
	
Universal endpoint agent that was developed by Facebook in 2014.
		
operating system instrumentation framework that exposes an operating system as a high-performance relational database. Using SQL, you can write a single query to explore any given data, regardless of the operating system.
		
	
Moloch (Arkime)
	
Enhances security infrastructure by storing and indexing network traffic in standard **PCAP** format, allowing for quick access. It features a user-friendly web interface for browsing, searching, and exporting PCAP files. Arkime provides APIs for downloading PCAP and JSON session data, and it supports integration with tools like **Wireshark** for analysis. Designed for scalability, Arkime can handle tens of gigabits per second of traffic, with PCAP and metadata retention adjustable based on available disk space and Elasticsearch cluster size. More information is available on the [Arkime GitHub](https://github.com/aol/moloch).
	
Log Aggregation

Syslog
A standard logging protocol. Network administrators can set up a Syslog server that receives logs from multiple systems, storing them in an efficient, condensed format that is easily queryable. Log aggregators can directly read and process Syslog data.

Event Streaming
Protocols like SNMP, Netflow, and IPFIX allow network devices to provide standard information about their operations, which can be intercepted by the log aggregator, parsed, and added to central log storage.

Log Collectors
Software agents that run on network devices, capture log information, parse it and send it to a centralized aggregator component for storage and analysis.

Direct Access
Log aggregators can directly access network devices or computing systems, using an API or network protocol to directly receive logs. This approach requires custom integration for each data source.
	
Data Types
	
Structured data: These are usually logs for Apache, IIS, Windows events, Cisco logs, and some other manufacturers. They have clearly-defined fields (such as “src_ip“) and are similar to other structured logs, making them relatively easy to parse and normalize.
Unstructured data: This type of logging typically comes from a custom-built application where each message can be printed differently in different operations and the event itself can span multiple lines with no defined event start point, event endpoint, or both. This is likely to be the majority of the data being sent to the SIEM.
