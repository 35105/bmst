# Threat Intelligence

### Threat Intelligence Types
**Strategic Threat Intelligence**

Definition: High-level, non-technical intelligence aimed at decision-makers, aiding in budget and policy decisions.
Examples:
Presentations linking global events to cyber activity (e.g., increased phishing during the Coronavirus pandemic).
Reports identifying patterns in cyber attacks over time (e.g., DDoS attacks occurring on Mondays).
Updates for internal security teams about threats targeting similar organizations (e.g., banks monitoring attacks against other banks).
Focus Areas:
Geopolitical situations and motives of countries that may pose threats.
Industry-specific cyber attack trends, particularly relevant to the organization's sector.

**Operational Threat Intelligence**

Definition: Intelligence focused on understanding threat actors targeting the organization, including their motivations and tactics.
Purpose: To build effective defenses by monitoring adversary techniques and gaining deeper insights into malicious groups.
Characteristics:
Typically technical and requires human analysts for tracking and research.
Not easily automated.

**Tactical Threat Intelligence**

Definition: Technical intelligence that provides immediate value, often shared as indicators of compromise (IOCs).
 Examples:
Lists of email addresses used for phishing (e.g., Emotet malware) for manual checks by analysts.
 Subscriptions to threat feeds with updated lists of malicious IPs for automated blocking by security systems.
Public reports containing IOCs related to exploitation of new vulnerabilities.
Usage: IOCs can be checked manually by analysts or integrated into security tools via APIs or threat feeds.

### Operational Threat Intelligence

STIX and TAXII are common methods of sharing threat intelligence.

**STIX:**

Structured Threat Information eXpression, or STIX, was developed by MITRE and the OASIS Cyber Threat Intelligence Technical Committee as a standardized language for sharing threat information. For some organizations and information-sharing committees, this has been the standard and is widely used. Whilst STIX is designed to be used in conjunction with TAXII, it can be shared without it. STIX is designed to share not just IOCs, but also threat:

Motivations
Abilities
Capabilities
Response

You can read more about STIX at this link – https://oasis-open.github.io/cti-documentation/stix/intro.html

MITRE have also provided some examples of indicators in STIX format, so you can get a feel of what STIX looks like here – https://stix.mitre.org/language/version1.0.1/samples.html

 
**TAXII:**

Trusted Automated eXchange of Intelligence Information, or TAXII, defines how cyber threat information can be shared by using services and message exchanges. Designed to handle STIX information, this platform is run on a server and allows the sharing of information between specified groups or provides a public “threat stream” that individuals can sign up to and receive intelligence.

### Tactical Threat Intelligence

THREAT EXPOSURE CHECKS

The threat intelligence team received an alert from US-CERT about increased exploitation activity related to "Vulnerability X," including a list of IP addresses involved in scanning for vulnerable devices. The assigned analyst will search for these IPs in the organization's SIEM platform, focusing on logs from the past 7 days to determine if any have scanned the organization's public IP range. If any malicious IPs are found, the organization may consider blocking them and set up alerts for future scanning attempts.

Collaboration between the threat intelligence team and the vulnerability management team is crucial, as understanding the context of vulnerabilities is essential. A high-rated vulnerability may not be actively exploited, while a medium-rated one could be. If a vulnerability is being actively exploited, it justifies immediate patching efforts.



### Types of Intelligence

**SIGINT**

Signal intelligence (SIGINT) involves intercepting radio signals and communications for intelligence, dating back to World War I. It includes:

**COMINT** – Communications intelligence focused on messages and voice communications, often synonymous with SIGINT.

**ELINT** – Electronic intelligence from non-communication systems, like missile guidance and radar.

These methods are commonly used in electronic warfare via drones and communications interception between governments.

**OSINT**

Open-source intelligence (OSINT) is gathered from public sources, including driving records, phone numbers, addresses, social media information, and email addresses.

**HUMINT**

Human intelligence (HUMINT) is collected from people. It requires understanding human behavior and is obtained through meetings, observations, and document gathering, often involving espionage or diplomatic communications.

**GEOINT**

Geospatial intelligence (GEOINT) supports operations during disasters, wartime, and political events. It uses satellite imagery to identify targets, structures, and military positions, aiding coordination and disaster response efforts.

### Threat Intelligence Platforms

**Malware Information Sharing Platform (MISP)**

Website: https://www.misp-project.org/
MISP is an open-source, community-ran project, developed and maintained by an awesome group of volunteers. MISP is used by over 6000 organizations around the world, and has been designed to be as simple as possible, making it accessible and usable. MISP offers an absolute ton of features providing extended functionality for multiple use-cases, including the ability to easily share intelligence with fellow humans, and even automated defenses.

**ThreatConnect**

Website: https://threatconnect.com/solution/threat-intelligence-platform/
ThreatConnect have produced their own Threat Intelligence Platform that can completely automate the intelligence collection process, regardless of the source format. Whether it’s an email, RSS feed, or blog, ThreatConnect can ingest it and store the intelligence within the TIP. ThreatConnect also provides automation in the form of runbooks, allowing human analysts to determine what actions should be taken under specific circumstances.

**Anomali**

Website: https://www.anomali.com/
The TIP produced and maintained by Anomali is utilized by many different Information Sharing and Analysis Centers including the Financial Services Information Sharing and Analysis Center (FS-ISAC). Anomali offers the ability for an organization to quickly and easily create its own ISAC, allowing other organizations to partner together and share intelligence together. The website also offers an “app store” where organizations can purchase integrations and threat feeds to boost the capabilities or the TIP and other security controls utilized by the organization.

**ThreatQ**

Website: https://www.threatq.com/threat-intelligence-platform/
The ThreatQ platform is based on a threat-centric approach to security operations, allowing security teams to “prioritize based on threat and risk, collaborate across teams, automate actions and workflows and integrate point products into a single security infrastructure”. ThreatQ also states that it can do more than a typical TIP, and can assist with security practices such as Vulnerability Management, Spear Phishing, Incident Response, and Threat Hunting.

# Threat Intelligence Lifecycle

1) Planning & Direction

    Importance: Establishes the scope and goals of the threat intelligence project.
    Stakeholders: Clearly define who is involved to keep the project focused and efficient.
    Example Activities:
        Research the hacking group (members, skills, sophistication).
        Assess public exposure to understand the organization's attack surface.
        Identify defensive actions against the identified threat.

2) Collection

    Objective: Gather necessary data to create actionable intelligence.
    Methods:
        Scrape posts from underground forums.
        Collect information on forum user accounts.
        Conduct OSINT searches for additional data.
    Tools: Utilize centralized threat intelligence platforms (e.g., MISP) for storing indicators and actionable intelligence.

3) Processing

    Goal: Transform collected data into a clear, readable format for analysis.
    Example: Translate non-English posts to maintain accuracy and clarity for analysts.

4) Analysis

    Process: Convert processed information into actionable intelligence.
    Decision-Making: Determine whether to investigate threats, block attacks, or enhance security measures.
    Presentation: Tailor the intelligence output to the audience:
        Technical details for security analysts.
        Simplified, business-focused insights for executive boards.

5) Dissemination

    Purpose: Distribute finished intelligence to relevant stakeholders (SOC, analysts, executive board).
    Considerations:
        Identify the intelligence needs of each audience.
        Determine the best presentation format for clarity and actionability.
        Establish frequency of updates and communication channels.
        Plan for follow-up on questions.

6) Feedback

    Significance: Ensure alignment with the needs of security teams consuming the intelligence.
    Guidance: Feedback informs:
        Data collection types.
        Processing and enrichment methods.
        Analysis and presentation strategies.
        Dissemination priorities and response times.
    Regular Updates: Adapt to changing requirements and priorities through continuous feedback.

