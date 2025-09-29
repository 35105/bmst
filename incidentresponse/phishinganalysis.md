# Phishing Anaysis

### Visualisation Tools
screenshot malicious sites - https://www.url2png.com/
screenshot malicious sites - https://urlscan.io/

### URL Reputation
scan file/url - https://www.virustotal.com/gui/home/upload
scan url - https://urlscan.io/

threat feed - https://urlhaus.abuse.ch/ - can create lists
threat feed - https://www.phishtank.com/

### File Reputation
https://www.virustotal.com/gui/home/upload - file/url
https://talosintelligence.com/talos_file_reputation - uses hashes

### Sandboxing
https://hybrid-analysis.com/ - file/url

### Other Sites 

virtual browser - https://www.wannabrowser.net/ - http response code
ip addr lookup - https://whois.domaintools.com/
decoding - https://cyberchef.io/
screenshot malicious sites - https://www.url2png.com/
screenshot malicious sites - https://urlscan.io/
threat feed - https://urlhaus.abuse.ch/ - can create lists
threat feed - https://www.phishtank.com/
file rep - https://www.virustotal.com/gui/home/upload - file/url
file rep - https://talosintelligence.com/talos_file_reputation - uses hashes
sandboxing - https://hybrid-analysis.com/ - file/url

# Immediate Response Process

1. **Retrieve Original Email**: Obtain the suspicious email via security technology, email servers, or employee forwarding.

2. **Gather Artifacts**: Collect necessary artifacts from the original email for analysis and defense.

	- Email Sender (search "From")
	- Date (search "Date")
	- Recipient (search "To")
	- Subject Line (search "Subject")
	- Reply-To (search "reply")
	- Sending Server IP (search X-Sender-IP")

3. **Inform Recipients**: Notify individuals who received the email using a template that includes:
   - Date and time sent
   - Subject line
   - Instructions (delete or forward)
   - Contact details for support

4. **Artifact Analysis**: Investigate email, web, and file artifacts using tools like sandboxing, VirusTotal, and others.

5. **Defensive Measures**: Implement actions to mitigate risks, such as blocking malicious URLs and emails.

6. **Complete Investigation Report**: Document all steps taken for an audit trail of the response process.

# Defensive Actions

## Marking External Emails

In the Exchange Admin Centre, navigate to **Mail Flow** and create a new email rule:
- **Condition**: If the sender is outside the organisation and the recipient is inside.
- **Action**: Prepend the subject line with **[EXTERNAL]**.

## Email Security Technology

- **SPF**: A text record that helps prevent email address forgery. It consists of three parts: record type, IP addresses and external domains authorised to send on behalf of the domain, and an enforcement rule. Example: `v=spf1 a:include:mailgun.org protection.outlook.com -all` (hard fail for unauthorised senders).

- **DKIM**: Cryptographically verifies that an email is sent from trusted servers. An encrypted hash of the email content is generated using a private key and added to the email header as a DKIM signature. Example: `v=DKIM1 <key type> <public key>`.

- **DMARC**: Allows domain owners to specify actions for emails failing both SPF and DKIM checks. Options include none, quarantine, and reject. Example: `v=DMARC1; p=quarantine; rua=mailto:contact@securityblue.team`.

## Spam Filter

- **Content Filter**: Analyses email headers and body to identify spam.
- **Rule-Based Filter**: Filters emails based on predefined criteria.
- **Bayesian Filter**: Uses machine learning to adapt to user spam preferences.

## Attachment Filtering

Blocks potentially harmful file types, including:
- .exe (Executable)
- .vbs (Visual Basic Script)
- .js (JavaScript)
- .iso (Optical Disk Image)
- .bat (Windows Batch File)
- .ps/.ps1 (PowerShell Scripts)
- .htm/.html (Web Pages)

## Attachment Sandboxing

Extracts and analyses email attachments in a virtual environment, monitoring their behaviour.

## Blocking Email Artefacts

- **Email Sender**: Bi-directional block at the gateway.
- **Sender Domain**: Caution, as this may block legitimate emails (e.g., Gmail, Outlook).
- **Sending Server IP**: Drop emails from specific IPs only when necessary.
- **Subject Line**: Block emails with specific subject lines.

## Blocking Web Artefacts

- **URL Blocks**: Specific to provided URLs; assess if the URL is static or has a malicious directory.
- **Domain Blocks**: Prevent access to an entire domain.
- **DNS Blackholing**: Create a fake DNS entry to redirect malicious URLs.
- **Firewall**: Block access to servers; typically used for scanning or attacking IPs, not common for phishing.

**Decision Criteria**: 
- If created with malicious intent (e.g., young age, no legitimate content), implement a **DOMAIN BLOCK**.
- If a compromised domain may be needed for business, request a **URL block**.

## Blocking File Artefacts

- **Hash Blocking**: Block MD5, SHA1, or SHA256 hashes using the organisation’s EDR tool to recognise and delete malicious files. If the AV solution doesn’t flag a file, the hash can be submitted to the vendor. Note that commodity/polymorphic malware may evade hash blocks.
- **Blocking File Names**: Rarely used; can generate watchlists of IOCs/artefacts for monitoring. File hashes are preferred for blocking malicious files.
