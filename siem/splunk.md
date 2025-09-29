# Splunk

Search Queries

index="botsv1" earliest=0 - index="botsv1" means search against that dataset, earliest=0 means start from the first event, couls also change date range to all time.
event sampling shows 1 per x number of events
sourcetype=fgt_traffic -- can select source types from sourcetype field menu, this is fortigate firewalls
	
field value
simple search for field and value
	
eg: search src="10.10.10.50" - search for this source ip
search src="10.10.10.50" OR dst="10.10.10.50", search for this source or this destination
search dst="10.10.100.5" - in ddos scenario, search destination to see traffic directed toward web server
		
Wildcards
use asterisk to mean anything
	
search src="10.10.10.73" dst="10.10.10.*" - 10.10.10.73 is compromised, check all other addresses it communicated with
search pass* AND fail* - search for logs relating to login fail, will return any logs containing pass fail, password fail, pass failure, password failure etc.
		
Searching for processes
	
index="botsv1" earliest=0 Image="*\\cmd.exe" | stats values(CommandLine) by host - Image field in Sysmon events shows the executable that has generated the process, sort per host
		
Additional Resources

https://docs.splunk.com/Documentation/Splunk/9.0.1/SearchTutorial/Startsearching
https://www.youtube.com/watch?v=xtyH_6iMxwA
	
Search Queries 

Time:
		| sort time asc
		| sort limit=2 time asc - first two events from search query
		desc for descending
	
Stats:
	
stats count by srcip - lists how many times something occured
sort count desc - sorth the count by desc
eg: index="botsv1" sourcetype=fortigate_utm "date=2016-08-19" | stats by count srcip | sort count desc
		
Table:
	
create a table of data to display
| table date, time, srcip, dstport, action, msg - will show all included fields
		
Uniq:
	
retrieve unique values
| table srcip | uniq - will show unique ip addresses
where uniq doesnt work dedup can be used - | table action | dedup action
		
Creating Alerts

1. Search Query - What activity should create the alert?
	
2. Search timing - How often will Splunk search? Continuously (majority of SIEM rules) or run on a schedule (usually used to identify changes from baselines and behavioral profiles)
	
3. Alert trigger - How many times can this happen before alert is generated? eg failed passwords shouldn't be one or it would always be alerting.
	
4. Alert action - What happens when an alert is triggered? Could send email to senior, add to recent list, log and index searchable alert events.
	
Create Rules

Make search query, click ‘Save As’ and then ‘Alert’.
Private - only you can access edit and view OR Shared in App - all users can view, have read access, power user has write access.
Alert type - scheduled or real time
trigger actions - trigger alert, log event, output to lookup file, run script or output to endpoint to notify SOC analyst.
	
Create Dashboard

Create Report: Search Query "Save as" "report"
	
Naming Convention: <group>_<object>_<description>
group: the name of the group or department using the knowledge object such as sales, IT, finance, etc.
object: report, dashboard, macro, etc.
description: WeeklySales, FailedLogins, etc.
		
Go to report and select add to dashboard
	
Dashboard Title – Set an optional human-readable name for the dashboard.
Dashboard ID – Set an identification number for the dashboard.
Dashboard Description – Set an optional description of what the dashboard’s intended purpose is.
Dashboard Permissions – It’s usually a good idea to keep the permissions set to Private until the dashboard has been tested.
Panel Title – Set an optional name for the panel within a dashboard.
Panel Powered By – Select the search query that powers the panel, either by writing a query in the ‘Inline Search’ box, or clicking on ‘Report’ and finding your saved report.
		
Click on your home app, select Choose a home dashboard
