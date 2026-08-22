# Multi-Stage SOC Investigation — Endpoint Compromise and Suspected DNS Exfiltration

**Platform:** TryHackMe SOC simulation  
**Date:** 22 August 2026  
**Primary host:** `win-3450`  
**SIEM:** Splunk Enterprise  
**Telemetry:** Sysmon, PowerShell and email events

## Executive Summary

This case study documents six SOC alerts investigated in a controlled security simulation. The alerts were analysed individually and correlated where the evidence showed they formed part of a broader sequence on `win-3450`. The investigation demonstrates phishing triage, process-tree analysis, PowerShell investigation, network-share activity analysis, data staging, Active Directory reconnaissance, suspicious DNS activity, incident classification, escalation decisions and remediation planning.

A key analytical finding was that several detections should not be treated as unrelated alerts. Correlation of process ancestry, command-line telemetry, file creation and DNS activity showed a sequence involving PowerShell PID `3728`, network-share access, staged data, PowerView activity and repeated `nslookup.exe` queries to encoded-looking subdomains of `haz4rdw4re.io`.

---

## SOC 1 — Suspicious External Email / Phishing

**Alert ID:** 1000  
**Severity:** Low  
**Incident type:** Phishing  
**Classification:** **True Positive**  
**Escalation:** **No**

An unsolicited inbound email from `eileen@trendymillineryco.me` used a fraudulent inheritance claim to persuade `support@tryhatme.com` to provide banking details. SIEM searches confirmed the suspicious message and no legitimate business purpose was identified. The message contained no attachment, and the available evidence did not show recipient interaction or disclosure of banking information.

**Attack indicators:** `eileen@trendymillineryco.me`, `trendymillineryco.me`, subject `Inheritance Alert: Unknown Billionaire Relative Left You Their Hat Fortunes`.

**Recommended response:** Block the sender and associated domain, quarantine/delete the message, notify the affected user, reinforce phishing-awareness guidance and monitor for related indicators.

---

## SOC 2 — Network Drive Mapping and Suspicious Data Staging

**Alert ID:** 1022  
**Severity:** Medium  
**Incident type:** Execution  
**Classification:** **True Positive**  
**Escalation:** **Yes**

Sysmon telemetry showed `powershell.exe` PID `3728` spawning `net.exe` to map drive `Z:` to `\\FILESRV-01\SSF-FinancialRecords`. The same PowerShell process was associated with creation of a staged ZIP archive under `C:\Users\michael.ascot\Downloads\exfiltration\`. The mapped drive was later removed using `net.exe use Z: /delete`. In context, the sequence was inconsistent with routine administrative activity and indicated suspicious access and staging behavior.

**Recommended response:** Isolate `win-3450`, investigate PowerShell PID `3728` and its full process tree, preserve and analyse the staged archive, identify files accessed through the mapped share, review authentication/network telemetry and determine whether data was transferred externally.

---

## SOC 3 — Repeated Nslookup Activity / Suspected DNS Exfiltration

**Alert ID:** 1025  
**Severity:** High  
**Incident type:** Process  
**Classification:** **True Positive**  
**Escalation:** **Yes**

PowerShell PID `3728` repeatedly spawned `nslookup.exe`. Splunk analysis identified ten distinct command lines containing encoded-looking/random strings as subdomains of `haz4rdw4re.io`. The activity correlated with the earlier staged archive and was assessed as suspected DNS-based data exfiltration.

**Attack indicators:** `win-3450`, `powershell.exe` PID `3728`, `nslookup.exe`, `haz4rdw4re.io`, repeated encoded-looking DNS queries and the staged archive.

**Recommended response:** Isolate the endpoint, block the suspicious domain at DNS/firewall controls, preserve relevant logs and artifacts, investigate the staged data, review DNS activity for other affected systems, perform endpoint forensic analysis and reset potentially compromised credentials where appropriate.

---

## SOC 4 — PowerView PowerShell Reconnaissance

**Alert ID:** 1020  
**Severity:** Low  
**Incident type:** Execution  
**Classification:** **True Positive**  
**Escalation:** **Yes**

A PowerShell script named `PowerView.ps1` was created at `C:\Users\michael.ascot\Downloads\PowerView.ps1`. Splunk evidence showed activity associated with execution rather than simple file presence. PowerView can be used to enumerate Active Directory users, computers, groups, trusts and related domain information. Within the broader suspicious sequence on `win-3450`, this was treated as reconnaissance activity requiring escalation.

**Recommended response:** Isolate the endpoint, terminate suspicious PowerShell activity, quarantine `PowerView.ps1`, determine how it was introduced, review PowerShell/Sysmon/authentication telemetry, identify accounts and systems queried and conduct endpoint forensic analysis.

---

## SOC 5 — Network Drive Disconnection Following Data Staging

**Alert ID:** 1024  
**Severity:** Medium  
**Incident type:** Execution  
**Classification:** **True Positive**  
**Escalation:** **Yes**

Sysmon recorded `net.exe` PID `8004`, with parent `powershell.exe` PID `3728`, executing `net.exe use Z: /delete`. Viewed alone, disconnecting a mapped drive may be benign. Correlation with the previously mapped financial-records share and staged archive made this event significant as part of the same suspicious sequence and potential cleanup following data access/staging.

**Recommended response:** Isolate `win-3450`, terminate malicious PowerShell activity, preserve the staged archive, determine what was accessed through `Z:`, review network/share and authentication logs, block confirmed malicious infrastructure and investigate possible data exfiltration.

---

## SOC 6 — Suspicious Parent-Child Process / DNS Exfiltration Activity

**Alert ID:** 1027  
**Severity:** High  
**Incident type:** Process  
**Classification:** **True Positive**  
**Escalation:** **Yes**

Sysmon identified `powershell.exe` PID `3728` spawning `nslookup.exe` PID `5432` from the `C:\Users\michael.ascot\Downloads\exfiltration\` working directory. The child process queried an encoded/random-looking subdomain of `haz4rdw4re.io`. Correlation with the repeated DNS queries and staged archive supported suspected DNS-based exfiltration and an active endpoint compromise.

**Attack indicators:** `haz4rdw4re.io`, encoded/random subdomains, `nslookup.exe` PID `5432`, `powershell.exe` PID `3728`, staged ZIP archive, `C:\Users\michael.ascot\Downloads\exfiltration\`, host `win-3450`.

**Recommended response:** Immediately isolate `win-3450`; terminate malicious PowerShell activity; block `haz4rdw4re.io` at DNS/firewall controls; quarantine the staged archive; preserve logs and artifacts; investigate the contents/source of staged data; hunt for related DNS activity across other endpoints; and reset affected credentials if compromise is confirmed.

---

## Correlated Attack Timeline

1. Suspicious PowerShell activity is observed on `win-3450`.
2. PowerShell PID `3728` launches `net.exe` and maps a network share to drive `Z:`.
3. Data is staged into a ZIP archive under the Downloads `exfiltration` directory.
4. `net.exe use Z: /delete` removes the mapped drive.
5. `PowerView.ps1` activity indicates Active Directory reconnaissance on the same endpoint.
6. PowerShell repeatedly spawns `nslookup.exe` with encoded-looking subdomains of `haz4rdw4re.io`, consistent with suspected DNS-based exfiltration.

This correlation is the central analytical value of the case: individual alerts that might appear low or medium severity in isolation become materially more significant when reconstructed as a single attack chain.

## Skills Demonstrated

- SOC alert triage and classification
- Splunk SIEM searching and event correlation
- Sysmon process and file-event analysis
- Parent-child process-tree analysis
- PowerShell investigation
- Network-share activity investigation
- Data-staging identification
- DNS anomaly and suspected exfiltration analysis
- IOC identification and documentation
- Escalation decision-making
- Containment and remediation planning
- Multi-alert attack-chain reconstruction

## Evidence Structure

Screenshots collected during the investigation document the alert details, Splunk searches, process relationships, suspicious command lines, DNS-query analysis, case-report classifications, remediation recommendations and escalation decisions for SOC 1–SOC 6.

> **Lab disclaimer:** This case study documents hands-on activity performed in a controlled TryHackMe cybersecurity simulation. Systems, identities, domains and telemetry shown are simulation evidence and do not represent a real production compromise.
