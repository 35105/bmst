# Incident Response Plan

## Preparation
	
Develop response plans
	
Ensure resources needed by response team are ready for use
	
Train and evaluate performance to ensure team members are capable of completing duties
	
## Identification

Guidance on how to report, what information to gather
	
When?
Who discovered it?
How?
What was affected?
Does its affect operations?
What is the scope?
				
Criticality level: How fast does response need to be?
Impact level: How long will it impact the business?
			
## Containment

What actions should be taken?
		
Disconnect devices?
Power off systems?
		
How will this effect collection of evidence?
	
Would powering off lose volatile evidence eg memory?
		
Document guidelines and procedures for evidence acquisition
	
Restore from backups so systems can resume without infected machines
	
## Eradication

What happened? Analyse
	
Work backward from MITRE ATT&CK
Review SIEM logs
		
Remove malicious artifacts
	
Malware?
System changes?
Remove persistance?
		
Harden systems
	
Apply patches
Empower automated defense with indicators of compromise found
Create runbooks
		
## Recovery

Move affected systems back into production
	
Scan etc, ensure they're no longer infected
		
## Lessons Learned

Hold a meeting including stakeholders
	
Recap what happened
What worked well?
How could response improve?
		
Should drive change, eg rewriting documentation, more budget for tools etc
