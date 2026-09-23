# Day 07 - Windows Failed Logon Investigation

## Overview

This portfolio entry documents a guided SOC Level 1 investigation of repeated Windows authentication failures. The investigation used synthetic and sanitized evidence to practice alert validation, Windows logon analysis, SIEM correlation, EDR process review, root-cause determination, remediation validation, and Five Ws reporting.

> \*\*Safety and scope:\*\* All accounts, hostnames, timestamps, and application names in the corporate scenario are fictional. No production system or live organization was accessed.

## Case Summary

|Field|Result|
|-|-|
|Case ID|`AUTH-2026-001`|
|Alert|Multiple Failed Windows Logins|
|Source|`NS-APP-02`|
|Destination|`NS-WKS-104`|
|Target account|`jrahman`|
|Activity|18 failed logons in 3 minutes|
|Event / Logon Type|`4625` / Type `3` Network|
|Detection verdict|True Positive|
|Activity classification|Benign authentication failure|
|Severity|Low|
|Status|Closed after remediation|

## Investigation Workflow

1. Confirmed the repeated failures using Event ID `4625`.
2. Identified Logon Type `3`, indicating network authentication rather than local console or RDP activity.
3. Mapped the source to an internal application server owned by IT Operations.
4. Searched for Event ID `4624` and found no matching successful logon from the same source and account during the investigation window.
5. Observed a regular 10-second retry interval, indicating automated activity.
6. Used simulated EDR evidence to identify an approved, signed `BackupAgent.exe` process running as a Windows service.
7. Correlated the failures with a recent password change and confirmed that the backup application retained stale stored credentials.
8. Verified that updating the stored credentials stopped the failures and restored the backup job.

## Key Evidence

|Evidence|Finding|
|-|-|
|Authentication failures|18 events over 3 minutes|
|Successful authentication|None from the same source/account in the search window|
|Retry interval|Every 10 seconds|
|Originating process|Approved `BackupAgent.exe`|
|Parent process|`services.exe`|
|Network service|SMB over TCP port `445`|
|Root cause|Old password remained in the backup configuration|
|Malicious indicators|None identified|

## Five Ws

**Who:** The `jrahman` account was used by `BackupAgent.exe` on `NS-APP-02` to authenticate to `NS-WKS-104`.

**What:** The application generated 18 failed Type 3 network logon attempts because it was using an old stored password.

**When:** The failures occurred between 9:14 AM and 9:17 AM EDT on September 22, 2026. The stored credentials were corrected at 9:27 AM EDT.

**Where:** The activity originated from `NS-APP-02` and targeted `NS-WKS-104` over SMB on TCP port `445`.

**Why:** `BackupAgent.exe` retained stale credentials after the account password changed and retried authentication every 10 seconds.

## Final Determination

The alert was a **true positive** because the repeated authentication failures occurred as detected. The underlying activity was **benign**, however, and resulted from an approved backup service using stale stored credentials. No successful unauthorized login, account compromise, malware, or suspicious network activity was identified.

## Remediation and Recommendations

* Updated the stored credentials and confirmed successful backup completion.
* Recommended replacing the personal user account with a dedicated service identity.
* Recommended a managed service account or approved credential vault where supported.
* Recommended a runbook for updating dependent services after password changes.
* Recommended monitoring for recurrence of the same authentication pattern.

## Skills Practiced

* Windows Event ID `4625` and `4624` correlation
* Logon Type interpretation
* Authentication timeline analysis
* SIEM alert triage
* EDR process and parent-process review
* True-positive versus malicious-activity classification
* Root-cause analysis and remediation validation
* Five Ws incident documentation

## Report

* [Professional Investigation Report](Day-07-Windows-Failed-Logon-Investigation-GitHub-Report.pdf)

## Learning Status

**Developing:** The investigation was completed accurately with structured guidance and report-writing templates.

