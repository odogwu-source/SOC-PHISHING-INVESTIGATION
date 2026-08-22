# SOC Phishing Investigation & Incident Triage

Hands-on phishing and email-security casework from controlled SOC simulations. This repository demonstrates alert triage, SIEM correlation, email authentication analysis, IOC identification, 5Ws reporting, True/False Positive classification, escalation decisions and remediation planning.

> **Portfolio note:** All cases are educational simulation evidence. They demonstrate analyst methodology and security judgement and are not presented as production incident-response engagements.

## Featured & Recent Casework

### Alert 1000 — Suspicious External Email / Inheritance Phishing
**22 August 2026 · Phishing · True Positive · No Escalation**

An unsolicited inheritance-themed email requested banking information. The investigation used subject, sender and recipient SIEM pivots to validate and scope the activity. The phishing attempt was confirmed, but available evidence did not establish user interaction or endpoint/account compromise, supporting remediation without escalation.

**Evidence:** 7 investigation screenshots covering initial alert, SIEM pivots, case reporting, classification and remediation.

[Open Alert 1000 case study →](cases/2026-08-22-suspicious-external-email/)

### Case 8817 — Microsoft Lookalike Phishing / Allowed Connection
**14 August 2026 · Phishing · True Positive · Escalated**

A Microsoft-themed phishing email used the lookalike domain `m1crosoftsupport.co`. SIEM and firewall correlation identified subsequent network activity from the recipient environment, with the connection allowed by the firewall. The potential for credential exposure or further endpoint activity supported escalation.

### Case 8816 — Amazon Delivery Phishing / Malicious URL Blocked
**14 August 2026 · Phishing · True Positive**

A phishing email impersonating Amazon delivery services contained a malicious shortened URL. Email, SIEM and firewall evidence correlated the message with an outbound access attempt, which was blocked by the firewall.

### Microsoft Support Impersonation / Suspicious Attachment
**Phishing · True Positive**

A simulated Microsoft Support message combined failed SPF/DKIM authentication, urgency, a dramatic pricing claim and a compressed `REPORT.rar` attachment. Correlated indicators supported a True Positive phishing classification.

## Analyst Workflow Demonstrated

1. Review alert context, severity and affected entity.
2. Examine sender, recipient, subject, URLs and attachments.
3. Validate SPF/DKIM and other available email-security indicators.
4. Pivot through SIEM using sender, subject, recipient, URL, IP and domain indicators.
5. Correlate email events with endpoint, firewall or network telemetry where available.
6. Document the incident using the 5Ws framework.
7. Classify the alert using evidence rather than assumption.
8. Determine whether escalation is proportionate to the observed impact.
9. Recommend containment, remediation and monitoring actions.

## Core Skills

**Phishing & Email Security:** phishing triage · social-engineering analysis · sender/domain analysis · SPF/DKIM analysis · URL and attachment assessment

**SOC & SIEM:** log searching · event correlation · IOC analysis · affected-entity scoping · firewall/network correlation · alert classification

**Incident Response:** 5Ws documentation · True/False Positive reasoning · escalation decisions · remediation planning · evidence preservation

---

## Case Archive

The sections below preserve the detailed write-ups and evidence for earlier investigations.

# Microsoft Support Impersonation — Suspicious Attachment

## Executive Summary

This project documents a hands-on SOC phishing investigation completed in a simulated security environment. The investigation involved analysing a suspicious email impersonating Microsoft Support, examining email authentication results, identifying phishing indicators, determining the affected user, classifying the alert, documenting findings using the 5Ws framework, and recommending appropriate remediation actions.

The alert was ultimately classified as a **True Positive phishing incident**.

## Alert Overview

| Field | Details |
| --- | --- |
| Alert Type | Phishing Email |
| Date & Time | 27 March 2025, 19:25 |
| Sender | Microsoft Support `support@microsoft.com` |
| Recipient | Eddie Huffman `e.huffman@tryhackme.thm` |
| Subject | Important Update: Microsoft Teams Pricing Increase |
| Attachment | `REPORT.rar` |
| URLs | None identified |
| SPF | Failed |
| DKIM | Failed |
| Final Verdict | **True Positive** |

## Investigation Summary

The email claimed Microsoft Teams pricing would increase by approximately 600%, creating urgency. SPF and DKIM both failed, weakening the legitimacy of the visible sender. The message also contained `REPORT.rar`, a compressed archive requiring scrutiny. Taken together, the impersonation, authentication failures, social-engineering language and attachment supported a True Positive verdict.

### 5Ws

**Who:** Microsoft Support (`support@microsoft.com`) appeared as the sender and Eddie Huffman (`e.huffman@tryhackme.thm`) was targeted.  
**What:** A suspicious pricing-themed email included `REPORT.rar`.  
**When:** 27 March 2025 at 19:25.  
**Where:** Simulated user email environment.  
**Why:** Failed SPF/DKIM, impersonation, urgency and the compressed attachment were consistent with phishing.

### Recommended Remediation

- Quarantine/remove the suspicious email.
- Prevent execution or extraction of the attachment.
- Determine whether the attachment was downloaded or executed.
- Review the affected account for suspicious activity.
- Search for similar messages delivered to other users.
- Block confirmed malicious indicators where appropriate.
- Escalate if additional compromise evidence is identified.

---

# SOC Investigation Case 8816 — Malicious URL Access / Phishing

## Incident Summary

A high-severity firewall alert was generated after internal source IP `10.20.2.17` attempted to access a blacklisted external URL. SIEM, firewall, email and URL evidence established that the URL originated from an inbound phishing email impersonating Amazon delivery services. The firewall blocked the connection and the case was classified as a **True Positive**.

## Key Evidence

- Source IP: `10.20.2.17`
- Destination IP: `67.199.248.11`
- Malicious URL: `http://bit.ly/3sHkX3da12340`
- Suspicious sender: `urgents@amazon.biz`
- Recipient: `h.harris@thetrydaily.thm`
- Subject: **Your Amazon Package Couldn't Be Delivered - Action Required**
- Firewall action: **Blocked**

### Investigation Evidence

![Initial Alert](images/Screenshot%202026-08-14%20143645.png)
![SIEM Investigation](images/Screenshot%202026-08-14%20144205.png)
![Email Investigation](images/Screenshot%202026-08-14%20144443.png)
![Firewall Telemetry](images/Screenshot%202026-08-14%20145452.png)
![Incident Classification](images/Screenshot%202026-08-14%20152559.png)
![Incident Closure](images/Screenshot%202026-08-14%20152642.png)

### Recommended Remediation

- Block the malicious URL and associated destination IP.
- Search email infrastructure for matching messages.
- Remove matching phishing emails.
- Review endpoint, firewall, proxy, DNS and email telemetry.
- Assess the affected endpoint for evidence of compromise.

---

# SOC Case 8817 — Inbound Email Containing Suspicious External Link

## Incident Summary

A medium-severity phishing alert identified an inbound message impersonating Microsoft Account Security. The email directed the recipient to `https://m1crosoftsupport.co/login`. SIEM correlation identified subsequent outbound network activity from endpoint `10.20.2.25` to `45.148.10.131` over TCP/443. Because the firewall action was **Allowed**, the case was classified **True Positive — Escalation Required**.

## Key Evidence

- Recipient: `c.allen@thetrydaily.thm`
- Sender: `no-reply@m1crosoftsupport.co`
- Domain: `m1crosoftsupport.co`
- URL: `https://m1crosoftsupport.co/login`
- Internal source IP: `10.20.2.25`
- Destination IP: `45.148.10.131`
- Destination port: `443`
- Firewall action: **Allowed**

### Investigation Evidence

![Suspicious URL Analysis](images/Screenshot%202026-08-14%20122606.png)
![Initial Alert](images/Screenshot%202026-08-14%20125141.png)
![SIEM Correlation](images/Screenshot%202026-08-14%20130028.png)
![Firewall Activity](images/Screenshot%202026-08-14%20130208.png)
![Incident Investigation](images/Screenshot%202026-08-14%20130517.png)
![Classification and Escalation](images/Screenshot%202026-08-14%20130539.png)

### Recommended Remediation

- Block the malicious domain, URL and associated destination IP.
- Isolate and investigate endpoint `10.20.2.25`.
- Determine whether credentials were entered or content downloaded.
- Reset credentials and revoke sessions if exposure is suspected.
- Verify MFA and monitor authentication activity.
- Search for additional recipients and remove matching messages.

---

## Portfolio Takeaway

These cases demonstrate progression from basic suspicious-email triage to multi-source SOC correlation and risk-based escalation. The key analytical principle throughout is **evidence over assumptions**: a phishing verdict establishes that a malicious message was delivered, while escalation depends on evidence of interaction, allowed malicious communication, credential exposure, endpoint activity or other material impact.

## Disclaimer

All identities, domains and telemetry shown in these case studies originate from controlled cybersecurity training/simulation environments. They are presented solely as educational portfolio evidence and do not represent real-world incidents involving the named brands or organisations.
