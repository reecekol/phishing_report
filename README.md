


# Phishing Incident Investigation & Response

View Full Report  [Here](https://example.com)


View Investigation Log  [Here](https://example.com)



## Overview

This project documents a phishing incident investigation conducted using Splunk in a simulated cybersecurity environment. The investigation focuses on identifying suspicious email activity, tracing indicators of compromise (IOCs), correlating email and web server logs, assessing potential user exposure, and developing incident response recommendations.

The objective was to reconstruct the phishing campaign timeline, determine the potential scope of the incident, and document appropriate containment and remediation measures.

- **Project Type:** Personal Cybersecurity Project
- **Role:** Security Analyst / Investigator
- **Tools:** Splunk, Search Processing Language (SPL), Email Logs, Web Server Logs
- **Investigation Period:** September–October 2026

## Objectives

- Investigate a suspected phishing email targeting an internal user.
- Trace suspicious email activity using Message-IDs and source IP addresses.
- Analyze web server logs to identify access to a suspected malicious URL.
- Correlate email recipients with website visitors to assess potential exposure.
- Evaluate evidence of possible credential compromise and unauthorized account activity.
- Document investigation findings, response actions, and security improvement recommendations.

## Tools & Technologies

| Tool / Technology | Purpose |
|---|---|
| Splunk | Search and analyze security logs |
| SPL | Query, filter, and correlate investigation data |
| Email Server Logs | Identify suspicious messages, senders, recipients, and Message-IDs |
| Web Server Logs | Investigate requests to the suspected phishing URL |
| Incident Response | Document findings, assess impact, and recommend remediation |

## Incident Scenario

The investigation began after the Security Operations Center (SOC) received a report of a suspicious email sent from an internal email account.

The investigation identified an initial suspicious email sent from an external address to an internal user. Subsequent activity indicated that the internal user's account was associated with additional phishing emails distributed to other employees.

The investigation focused on determining whether the initial email was associated with a credential-harvesting attempt, identifying users potentially exposed to the malicious website, and assessing the scope of the campaign.

> **Note:** This repository describes a personal investigation project using a simulated or training dataset. Findings represent the evidence available in that dataset and should not be interpreted as confirmation of a real-world security incident.

## Investigation Workflow

### 1. Email Log Analysis

Searched email records to identify suspicious messages sent to the initial recipient and reviewed relevant message metadata.

**SPL Query:**

```spl
index="p3-mail" to="brogers@northamericanlumbercoalition.com"
| table _time sender from to subject message_id src_ip
| sort _time
```

**Objectives:**
- Identify potentially malicious emails.
- Extract sender and recipient details.
- Record Message-IDs for further investigation.
- Identify relevant source IP addresses.

*Note: Field names may need adjustment to match the dataset's Splunk field extractions.*

### 2. Message-ID Investigation

Used the suspicious email's Message-ID to locate associated email records.

**SPL Query:**

```spl
index="p3-mail" "22A0C7DBBB"
```

This search supported further analysis of the suspicious email and helped establish the relationship between the identified message and the broader phishing activity.

### 3. Source IP Investigation

Searched the email dataset for messages associated with the suspicious source IP address.

**SPL Query:**

```spl
index="p3-mail" "85.189.238.171"
```

The investigation identified an earlier message associated with the same IP address and reviewed its sender, recipient, timestamp, and Message-ID.

**Additional Message-ID Search:**

```spl
index="p3-mail" "7B6BF0ECDC"
```

This provided additional context for investigating the possible relationship between the initial phishing attempt and subsequent suspicious account activity.

### 4. Web Server Log Analysis

Analyzed web server records to identify requests to the suspected phishing URL and investigate potential user interaction.

**SPL Query:**

```spl
index="p3-http" "10.10.0.23"
| table timestamp http.hostname http.url dest_ip
| sort timestamp
```

**Objectives:**
- Identify requests to the suspicious website.
- Establish the timing of web activity.
- Investigate potential connections between email delivery and website access.
- Identify users who may have interacted with the suspected phishing link.

*Note: Verify field names and confirm that the IP address represents the intended client in the dataset.*

### 5. Email and Web Log Correlation

Compared the list of users who received the suspected phishing email with the list of users who accessed the suspected malicious website.

This correlation was used to assess potential user exposure and investigate whether website activity was associated with the phishing campaign.

The analysis distinguished between email delivery, website access, and confirmed compromise. Website access alone was not treated as proof of credential theft or endpoint compromise.

## Key Findings

The investigation dataset produced the following preliminary findings:

| Finding | Result |
|---|---|
| Internal accounts targeted | Approximately 767 |
| Users recorded as visiting the suspicious website | Approximately 205 |
| Suspected malicious URL | `http://myers.com/login` |
| Suspicious source IP | `85.189.238.171` |
| Initial recipient investigated | Brenda Rogers |
| Confirmed credential theft | Not established by available evidence |

The figures describe the scope recorded in the investigation dataset. The relationship between recipients and website visitors should be validated through user-level correlation before calculating an exposure or click rate.

## Incident Response Actions and Recommendations

The investigation documented or recommended the following response measures. Each action should be identified as completed, in progress, or recommended based on the available evidence.

### 1. Email Containment

- Quarantine identified phishing emails.
- Search for and remove matching messages from affected mailboxes.
- Investigate related messages using Message-IDs, sender addresses, and other indicators.

### 2. Account Security

- Reset credentials for accounts with evidence of compromise.
- Revoke active sessions where appropriate.
- Investigate suspicious sign-ins, mailbox forwarding rules, and permission changes.
- Review multi-factor authentication (MFA) settings.

### 3. Threat Indicator Blocking

- Block validated malicious URLs and domains.
- Block relevant IP addresses and sender addresses where appropriate.
- Search other security data sources for related indicators.

### 4. Endpoint Investigation

- Review endpoint telemetry for suspicious downloads, processes, scripts, or malware activity.
- Identify systems associated with users who accessed the suspected malicious URL.
- Document endpoint investigation results.

### 5. Credential Exposure Assessment

- Identify users who accessed the suspected phishing website.
- Investigate whether credentials were submitted.
- Review subsequent authentication activity for signs of unauthorized access.
- Document unresolved questions and evidence limitations.

### 6. Continuous Monitoring

- Monitor email, identity, endpoint, DNS, and web proxy logs.
- Alert on unusual sign-ins and abnormal email-sending volumes.
- Validate that unauthorized activity has stopped after containment.

## Financial Impact

The preliminary response cost estimate was calculated as follows:

| Cost Category | Calculation | Estimated Cost |
|---|---|---:|
| Investigation | 20 hours × $125/hour | $2,500 |
| Remediation Labor | 10 hours × $150/hour | $1,500 |
| **Total** | | **$4,000** |

These figures represent preliminary estimated response costs and do not necessarily include indirect costs, business interruption, or potential financial losses.

## Lessons Learned

### Employee Security Awareness

Phishing emails can deceive employees and lead to account compromise. Regular training should help employees recognize suspicious links, identify credential-harvesting websites, and report phishing attempts promptly.

### Compromised Accounts Can Spread Phishing Internally

A compromised internal email account can be used to distribute additional malicious messages, potentially increasing the campaign's reach and making the messages appear more trustworthy.

### Early Detection and Reporting

Prompt reporting enables security teams to investigate malicious activity and initiate containment before additional users are affected.

### Coordinated Incident Response

Password resets and email containment are important, but response procedures should also address active sessions, suspicious sign-ins, credential exposure, and potential endpoint compromise.

### Accurate Impact Assessment

The investigation identified approximately 767 targeted accounts and 205 website visitors. The number of users who submitted credentials or experienced further compromise remained unconfirmed.

This demonstrates the importance of correlating reliable evidence before drawing conclusions about incident impact.

### Documentation and Cost Tracking

Maintaining accurate timelines, evidence, remediation records, and investigation hours supports incident reviews, management reporting, and future security improvements.

## Opportunities for Improvement

### Strengthen Email Security Controls

Review and improve email filtering, anti-phishing protections, and sender authentication controls, including SPF, DKIM, and DMARC.

### Enforce Multi-Factor Authentication

Require MFA for email and other critical accounts. Implement risk-based authentication controls where available to reduce the risk of unauthorized access following credential theft.

### Improve Security Monitoring

Configure alerts for unusual login locations, suspicious authentication attempts, abnormal email-sending volumes, and access to known malicious URLs.

Correlate email, identity, endpoint, DNS, and proxy logs to identify related activity more quickly.

### Enhance Incident Response Procedures

Maintain a phishing incident response playbook covering:

- Email quarantine and malicious message removal
- Account password resets and session revocation
- Endpoint investigation
- User notification
- Credential exposure assessment
- Post-containment validation

### Improve Exposure Assessment

Establish a repeatable process for identifying targeted recipients, validating link clicks, investigating potential credential submission, scanning affected endpoints, and determining whether other accounts or systems were compromised.

### Expand Security Awareness Training

Conduct regular phishing simulations and targeted training to help employees recognize malicious emails, avoid suspicious links, and report potential security incidents.

### Improve Threat Indicator Management

Record and validate malicious URLs, IP addresses, sender addresses, and email Message-IDs. Use these indicators to search across security systems for related activity and block confirmed threats where appropriate.

### Establish Remediation Ownership

Assign responsible teams and completion dates to each improvement. Track progress and verify effectiveness through security testing, monitoring reviews, and follow-up assessments.

## Deliverables

This repository can include the following artifacts:

- `README.md` — Project overview and investigation methodology.
- `incident-report.md` — Sanitized incident report with findings and recommendations.
- `spl-queries.md` — SPL queries used during the investigation.
- `timeline.md` — Chronological sequence of relevant events.
- `evidence/` — Sanitized screenshots of relevant Splunk searches and results.

## Skills Demonstrated

- Security Information and Event Management (SIEM)
- Splunk and SPL query development
- Email security analysis
- Web server log analysis
- Indicator of Compromise (IOC) investigation
- Log correlation and timeline reconstruction
- Phishing incident response
- Evidence-based impact assessment
- Technical documentation
- Security remediation planning

## Disclaimer

This repository is intended for educational and portfolio purposes.

All identities, indicators, domains, addresses, and investigation results should be treated as training data unless independently verified otherwise. No credentials, confidential organizational records, or sensitive production logs should be published in this repository.

## Author

**Kole Reece**

Personal cybersecurity project focused on phishing investigation, SIEM analysis, and incident response.

