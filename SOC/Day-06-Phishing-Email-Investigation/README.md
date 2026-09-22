# Day 06 - Phishing Email Investigation

## Overview

This portfolio entry documents a guided SOC Level 1 investigation of a fully synthetic Microsoft 365 credential-phishing email. The investigation followed a practical workflow: alert triage, sender and URL analysis, attachment review, email-authentication checks, user-impact assessment, containment, and Five Ws reporting.

> Safety: All domains use the reserved `.example` namespace and all URLs are defanged. No live malicious infrastructure was accessed.

## Case Summary

|Field|Result|
|-|-|
|Case ID|`PHISH-2026-001`|
|Verdict|Confirmed Credential Phishing|
|Severity|Medium|
|User Interaction|None|
|Observed Compromise|None|
|Final Status|Closed|

## Key Indicators

* Lookalike sender domain: `micros0ft-support.example`
* From/Reply-To mismatch
* Defanged URL: `hxxps://microsoft.login-check.example/verify`
* Suspicious attachment: `Microsoft\_Account\_Verification.html`
* SPF: Fail
* DKIM: None
* DMARC: Fail
* Urgency and account-suspension language

## Investigation Workflow

1. Identified the display name and actual sender address.
2. Detected a lookalike domain using `0` in place of `o`.
3. Compared the From and Reply-To domains.
4. Analyzed urgency and fear-based social engineering.
5. Parsed the URL and identified the actual main domain.
6. Assessed the HTML attachment as a credential-harvesting risk.
7. Interpreted SPF, DKIM, and DMARC results.
8. Confirmed the user did not click, open the attachment, or submit credentials.
9. Quarantined the message, blocked indicators, and searched other mailboxes.
10. Documented the incident using the Five Ws and closed the case.

## Five Ws

* **Who:** Targeted user `employee@northstarfinance.com`; reported sender `security@micros0ft-support.example`.
* **What:** Microsoft 365 impersonation using a misleading URL and HTML credential-harvesting attachment.
* **When:** Received at 10:15 AM, reported at 10:22 AM, and triage started at 10:25 AM EDT on September 21, 2026.
* **Where:** Northstar Finance Microsoft 365 email environment.
* **Why:** Correlated sender, URL, attachment, social-engineering, and authentication indicators confirmed phishing.

## Containment

* Quarantined the phishing message.
* Blocked the malicious sender, domains, URL, and relevant indicators.
* Performed an organization-wide mailbox search.
* Quarantined matching copies.
* Documented the findings, timeline, impact, and response actions.

## MITRE ATT\&CK Mapping

|Technique|ID|Evidence|
|-|-|-|
|Spearphishing Attachment|T1566.001|HTML attachment|
|Spearphishing Link|T1566.002|Misleading verification URL|
|Web Portal Capture|T1056.003|Suspected fake login page|

## Skills Demonstrated

* Phishing email triage
* Sender and domain analysis
* URL parsing and defanging
* HTML attachment risk assessment
* SPF, DKIM, and DMARC interpretation
* User-impact assessment
* Containment and mailbox scoping
* Five Ws incident reporting
* MITRE ATT\&CK mapping

\---

**Analyst:** MD Mostakim Hossain  
**Track:** AI + Cybersecurity / SOC Analyst Development

