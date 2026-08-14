# SOC Phishing Investigation & Incident Triage

## 📌 Executive Summary

This project documents a hands-on SOC phishing investigation completed in a simulated security environment.

The investigation involved analysing a suspicious email impersonating Microsoft Support, examining email authentication results, identifying phishing indicators, determining the affected user, classifying the alert, documenting findings using the 5Ws framework, and recommending appropriate remediation actions.

The alert was ultimately classified as a **True Positive phishing incident**.

---

## 🚨 Alert Overview

| Field         | Details                                            |
| ------------- | -------------------------------------------------- |
| Alert Type    | Phishing Email                                     |
| Date & Time   | 27 March 2025, 19:25                               |
| Sender        | Microsoft Support `<support@microsoft.com>`        |
| Recipient     | Eddie Huffman `<e.huffman@tryhackme.thm>`          |
| Subject       | Important Update: Microsoft Teams Pricing Increase |
| Attachment    | `REPORT.rar`                                       |
| URLs          | None identified                                    |
| SPF           | Failed                                             |
| DKIM          | Failed                                             |
| Final Verdict | **True Positive**                                  |

---

## 🎯 Investigation Objectives

The objectives of the investigation were to:

* Determine whether the alert represented a genuine security threat.
* Analyse the sender and email authentication results.
* Identify suspicious attachments or links.
* Determine the affected entity.
* Identify indicators associated with phishing.
* Classify the alert as a True Positive or False Positive.
* Document the investigation clearly.
* Recommend appropriate remediation actions.

---

## 🔎 Investigation Process

### 1. Alert Triage

I began by reviewing the security alert and examining the email metadata.

The message claimed to originate from:

**Microsoft Support**
`support@microsoft.com`

The recipient was:

**Eddie Huffman**
`e.huffman@tryhackme.thm`

The email claimed that Microsoft Teams pricing would increase by approximately **600%**, creating urgency for the recipient.

This unusually dramatic claim was treated as an initial social-engineering indicator.

---

### 2. Email Authentication Analysis

I reviewed the available email authentication information.

The investigation showed:

* **SPF: Failed**
* **DKIM: Failed**

These failures indicated that the email could not be successfully authenticated as legitimate Microsoft correspondence.

Although the visible sender appeared to use the `microsoft.com` domain, the failed authentication checks increased the likelihood of sender spoofing or impersonation.

---

### 3. Attachment Analysis

The email contained the following attachment:

`REPORT.rar`

RAR archives can be used to conceal or deliver potentially malicious files and therefore required additional scrutiny.

The combination of:

* A suspicious pricing claim
* Urgency/social engineering
* Failed SPF
* Failed DKIM
* A compressed RAR attachment

provided multiple indicators consistent with a phishing attempt.

---

## 👤 Affected Entity

The affected user identified during the investigation was:

**Eddie Huffman**
`e.huffman@tryhackme.thm`

This account was the recipient of the suspicious phishing email.

---

## 🧩 Indicators of Compromise / Suspicious Indicators

The investigation identified the following suspicious indicators:

| Indicator          | Finding                                        |
| ------------------ | ---------------------------------------------- |
| Sender             | `support@microsoft.com`                        |
| Attachment         | `REPORT.rar`                                   |
| SPF                | Failed                                         |
| DKIM               | Failed                                         |
| Social Engineering | Claimed ~600% Microsoft Teams price increase   |
| Urgency            | Designed to pressure the recipient into action |

> Note: These indicators originate from a simulated cybersecurity training environment and should not be interpreted as evidence of an actual Microsoft security incident.

---

## 📝 5Ws Analysis

### Who?

The email appeared to originate from **Microsoft Support (`support@microsoft.com`)** and targeted **Eddie Huffman (`e.huffman@tryhackme.thm`)**.

### What?

A suspicious email claimed that Microsoft Teams pricing would increase by approximately 600% and included a compressed attachment named `REPORT.rar`.

### When?

The alert occurred on **27 March 2025 at 19:25**.

### Where?

The suspicious activity was identified within the simulated user's email environment.

### Why?

The message was considered suspicious because:

* SPF authentication failed.
* DKIM authentication failed.
* The message used an unusually dramatic pricing increase to create urgency.
* A compressed RAR attachment was included.
* The sender appeared to impersonate Microsoft Support.

---

## ⚖️ Alert Classification

### Verdict: TRUE POSITIVE

The alert was classified as a **True Positive phishing incident**.

The decision was based on multiple correlated indicators rather than a single observation.

The strongest evidence included:

1. Failed SPF authentication.
2. Failed DKIM authentication.
3. Microsoft Support impersonation.
4. Suspicious social-engineering language.
5. The presence of a compressed `REPORT.rar` attachment.

---

## 🛡️ Recommended Remediation

Recommended defensive actions included:

* Quarantine or remove the suspicious email from the affected mailbox.
* Prevent users from opening or extracting the suspicious attachment.
* Investigate whether the attachment was downloaded or executed.
* Review the affected user's account for suspicious activity.
* Block confirmed malicious indicators where appropriate.
* Search the environment for similar messages delivered to other users.
* Escalate the incident for further investigation where required.
* Reinforce phishing-awareness guidance for users.

---

## 📚 Skills Demonstrated

This investigation demonstrates practical experience in:

* SOC Alert Triage
* Phishing Analysis
* Incident Response
* Security Operations
* Threat Detection
* Email Security Analysis
* IOC Identification
* Security Incident Documentation
* SPF/DKIM Analysis
* True Positive / False Positive Classification

---

## 🧠 Lessons Learned

This investigation reinforced the importance of analysing multiple pieces of evidence before determining whether a security alert represents a genuine threat.

A familiar sender name or legitimate-looking email address alone cannot establish authenticity. Email authentication mechanisms such as **SPF and DKIM**, attachment characteristics, social-engineering techniques, message context, and affected entities should be considered together.

The exercise also strengthened my ability to document SOC investigations using the **5Ws framework** and communicate findings in a structured manner.

---

## ⚠️ Disclaimer

This project documents work completed in a **simulated cybersecurity training environment** for educational and portfolio purposes.

The investigation does not represent an actual compromise of Microsoft, and no confidential corporate or personal production data is included.



---

# SOC Investigation Case 8816 — Malicious URL Access / Phishing

## Incident Summary

A high-severity firewall alert was generated after internal source IP `10.20.2.17` attempted to access a blacklisted external URL. Investigation of SIEM, firewall, email, and URL reputation data established that the URL originated from an inbound phishing email impersonating Amazon delivery services.

The suspicious URL was independently identified as malicious, and firewall telemetry confirmed that the connection attempt was blocked. Based on the correlated evidence, the incident was classified as a **True Positive**.

## Alert Details

- **Case ID:** 8816
- **Alert:** Access to Blacklisted External URL Blocked by Firewall
- **Severity:** High
- **Data Source:** Firewall
- **Source IP:** `10.20.2.17`
- **Source Port:** `34257`
- **Destination IP:** `67.199.248.11`
- **Destination Port:** `80`
- **Protocol:** TCP
- **Application:** Web Browsing
- **Firewall Action:** Blocked
- **Rule:** Blocked Websites
- **Malicious URL:** `http://bit.ly/3sHkX3da12340`
- **Activity Time:** `2026-08-14 13:54:59.234`

## Investigation — 5Ws

**Who:** Internal host `10.20.2.17`, associated during SIEM correlation with email recipient `h.harris@thetrydaily.thm`.

**What:** The host attempted to access a malicious shortened URL delivered through a phishing email. The firewall detected the request and blocked the connection.

**When:** The phishing email was observed at approximately `13:53:45`, followed by the malicious URL access attempt at `13:54:59` on 14 August 2026.

**Where:** Activity originated from internal source IP `10.20.2.17` and targeted external destination IP `67.199.248.11` over TCP port 80.

**Why:** SIEM correlation showed that the URL appeared in an inbound email claiming that an Amazon package could not be delivered and requesting the recipient to update shipping information. URL/IP analysis subsequently classified the URL as malicious.

## SIEM Investigation

SIEM searches were performed using the source IP, destination IP, malicious URL, and email-related indicators.

The investigation correlated:

- A phishing email containing the suspicious shortened URL.
- An inbound message sent from `urgents@amazon.biz`.
- Recipient `h.harris@thetrydaily.thm`.
- Subject: **Your Amazon Package Couldn't Be Delivered - Action Required**
- A subsequent firewall connection attempt from `10.20.2.17`.
- Destination `67.199.248.11`.
- The firewall successfully blocking the malicious URL request.
### Investigation Evidence

The following screenshots document the SIEM investigation and correlation of the phishing email, malicious URL, affected host, and firewall activity.

#### Evidence 1 – Initial Alert / Case Review

![Initial Alert](images/Screenshot%202026-08-14%20143645.png)

#### Evidence 2 – SIEM Investigation

![SIEM Investigation](images/Screenshot%202026-08-14%20144205.png)

#### Evidence 3 – Email and URL Correlation

![Email Investigation](images/Screenshot%202026-08-14%20144443.png)

#### Evidence 4 – Firewall Telemetry

![Firewall Telemetry](images/Screenshot%202026-08-14%20145452.png)

#### Evidence 5 – Investigation and Incident Classification

![Incident Classification](images/Screenshot%202026-08-14%20152559.png)

#### Evidence 6 – Final Incident Report / Closure

![Incident Closure](images/Screenshot%202026-08-14%20152642.png)

---
## Classification

**True Positive**

The alert was classified as a True Positive because multiple independent data sources corroborated malicious activity. The suspicious URL was present in an inbound phishing email, URL analysis classified it as malicious, and firewall telemetry confirmed an attempted connection to the associated destination.

## Indicators of Compromise / Attack Indicators

- Malicious URL: `http://bit.ly/3sHkX3da12340`
- Destination IP: `67.199.248.11`
- Suspicious sender: `urgents@amazon.biz`
- Internal source IP: `10.20.2.17`
- Phishing subject: **Your Amazon Package Couldn't Be Delivered - Action Required**

## Recommended Remediation

- Block the malicious URL and associated destination IP across applicable security controls.
- Search email infrastructure for additional messages from the suspicious sender or containing the same URL.
- Remove matching phishing emails from affected mailboxes.
- Review endpoint, firewall, proxy, DNS, and email telemetry for additional related activity.
- Assess the affected endpoint for evidence of compromise.
- Reinforce phishing-awareness guidance for affected users.

## Outcome

The malicious connection attempt was blocked by the firewall, the alert was investigated and correlated with phishing activity, and Case 8816 was closed as a **True Positive**.


---

# SOC Case 8817 – Inbound Email Containing Suspicious External Link

## Alert Overview

- **Case ID:** 8817
- **Alert:** Inbound Email Containing Suspicious External Link
- **Severity:** Medium
- **Category:** Phishing
- **Timestamp:** 14 August 2026 at approximately 09:50
- **Recipient:** `c.allen@thetrydaily.thm`
- **Sender:** `no-reply@m1crosoftsupport.co`
- **Subject:** **Unusual Sign-In Activity on Your Microsoft Account**
- **Attachment:** None
- **Direction:** Inbound

## Investigation – 5Ws

**Who:**  
The phishing email targeted `c.allen@thetrydaily.thm`. Subsequent network activity was observed from internal endpoint `10.20.2.25`.

**What:**  
An inbound email impersonating Microsoft Account Security contained a suspicious external link directing the recipient to `https://m1crosoftsupport.co/login`. SIEM investigation identified a subsequent outbound web connection associated with the phishing URL.

**When:**  
The email event occurred on 14 August 2026 at approximately `09:50:02`, followed by network activity at approximately `09:51:11`.

**Where:**  
The phishing email was delivered to `c.allen@thetrydaily.thm`, and subsequent activity originated from internal endpoint `10.20.2.25` and connected to destination IP `45.148.10.131` over TCP port `443`.

**Why:**  
The email used a lookalike Microsoft domain and an unusual sign-in warning to create urgency and persuade the recipient to follow the embedded link. SIEM correlation showed that the internal endpoint subsequently accessed the associated infrastructure.

## SIEM Investigation

SIEM searches were performed to correlate the suspicious sender/domain, URL, internal source IP, and destination IP.

The investigation identified:

- Suspicious sender: `no-reply@m1crosoftsupport.co`
- Suspicious domain: `m1crosoftsupport.co`
- Phishing URL: `https://m1crosoftsupport.co/login`
- Internal source IP: `10.20.2.25`
- Destination IP: `45.148.10.131`
- Destination port: `443`
- Protocol: TCP
- Application: Web Browsing
- Firewall action: **Allowed**
- Firewall rule: `Allow-Internet`

The SIEM evidence therefore demonstrated correlation between the inbound phishing email and subsequent network activity from the recipient's environment.

## Investigation Evidence

### Evidence 2 – Suspicious URL Analysis

![Case 8817 Evidence 1](images/Screenshot%202026-08-14%20122606.png)

### Evidence 1 – Initial Alert / Email Review

![Case 8817 Evidence 2](images/Screenshot%202026-08-14%20125141.png)

### Evidence 3 – SIEM Correlation

![Case 8817 Evidence 3](images/Screenshot%202026-08-14%20130028.png)

### Evidence 4 – Firewall / Network Activity

![Case 8817 Evidence 4](images/Screenshot%202026-08-14%20130208.png)

### Evidence 5 – Incident Investigation

![Case 8817 Evidence 5](images/Screenshot%202026-08-14%20130517.png)

### Evidence 6 – Incident Classification and Escalation

![Case 8817 Evidence 6](images/Screenshot%202026-08-14%20130539.png)

## Classification

**True Positive – Escalation Required**

The alert was classified as a **True Positive** because the suspicious inbound email contained a phishing URL using a Microsoft lookalike domain, and SIEM/firewall telemetry showed subsequent network activity from internal endpoint `10.20.2.25` to the associated destination.

The firewall action was **Allowed**, increasing the potential risk of credential exposure or additional malicious activity. The incident therefore required escalation for further endpoint and account investigation.

## Indicators of Compromise / Attack Indicators

- **Suspicious sender:** `no-reply@m1crosoftsupport.co`
- **Suspicious domain:** `m1crosoftsupport.co`
- **Malicious URL:** `https://m1crosoftsupport.co/login`
- **Destination IP:** `45.148.10.131`
- **Internal source IP:** `10.20.2.25`
- **Destination port:** `443`
- **Phishing subject:** **Unusual Sign-In Activity on Your Microsoft Account**

## Recommended Remediation

- Block `m1crosoftsupport.co`, the malicious URL, and associated destination IP across applicable security controls.
- Isolate and investigate endpoint `10.20.2.25`.
- Review browser, endpoint, DNS, proxy, and firewall telemetry for additional malicious activity.
- Determine whether the user entered credentials or downloaded any content.
- Reset the affected user's credentials and revoke active sessions if credential exposure is suspected.
- Enable or verify MFA for the affected account.
- Search the email environment for additional recipients of the phishing message.
- Remove matching phishing emails from affected mailboxes.
- Monitor for suspicious authentication activity associated with the affected account.

## Outcome

Case 8817 was classified as a **True Positive phishing incident**. Unlike the previous blocked connection, the network connection associated with this incident was **allowed**. Due to the possibility of credential compromise or additional endpoint activity, the case was **escalated for further investigation**.


---

# SOC Case 8814 – Inbound Email Containing Suspicious External Link

## Alert Overview

- **Alert ID:** 8814
- **Alert Type:** Inbound Email Containing Suspicious External Link
- **Severity:** Medium
- **Category:** Phishing
- **Timestamp:** 08/14/2026 09:44:31.285
- **Sender:** onboarding@hrconnex.thm
- **Recipient:** j.garcia@thetrydaily.thm
- **Subject:** Action Required: Finalize Your Onboarding Profile
- **Attachment:** None
- **Final Classification:** **False Positive**
- **Escalation Required:** No

## Executive Summary

Alert 8814 was generated after an inbound email containing an external link was delivered to an employee. The email instructed the recipient to complete an onboarding profile through HRConnex.

The alert was investigated using email telemetry, SIEM correlation, internal email correspondence, and URL reputation analysis. Investigation confirmed that HRConnex is an authorized third-party HR partner used by TheTryDaily for employee onboarding. The embedded URL was also analysed and returned a **CLEAN** result.

No evidence of phishing or malicious activity was identified. The alert was therefore classified as a **False Positive**.

## 5Ws Analysis

### Who?
- **Sender:** onboarding@hrconnex.thm
- **Recipient:** j.garcia@thetrydaily.thm
- The internal correspondence confirmed HRConnex as a third-party HR onboarding provider.

### What?
An inbound onboarding email containing an external URL triggered a phishing-related security alert.

### When?
The alert activity occurred on **August 14, 2026 at approximately 09:44 UTC**.

### Where?
The activity involved the recipient **j.garcia@thetrydaily.thm** and the external HRConnex onboarding infrastructure.

### Why?
The alert was triggered because the inbound email contained an external URL. Investigation established that the URL was associated with a legitimate employee onboarding process rather than phishing activity.

## Investigation Process

### 1. Initial Alert Review

The alert details were reviewed to identify the sender, recipient, subject, email content, and embedded external URL.

The email contained the following onboarding URL:

`https://hrconnex.thm/onboarding/15400654060/j.garcia`

### 2. SIEM Investigation

A SIEM search was performed for:

`hrconnex.thm`

The search returned email-related events associated with the onboarding activity.

Internal email correspondence showed that HRConnex was the organisation's third-party HR partner responsible for handling employee onboarding and account setup.

This provided important contextual evidence that the email was legitimate.

### 3. URL Reputation Analysis

The embedded onboarding URL was analysed using the URL/IP Security Check tool.

**Result: CLEAN**

No malicious reputation was identified for the URL.

### 4. Correlation and Validation

The email alert, SIEM events, internal correspondence, and URL reputation results were correlated.

The evidence supported legitimate business activity associated with employee onboarding rather than a phishing campaign.

## Related Entities

- `j.garcia@thetrydaily.thm`
- `onboarding@hrconnex.thm`
- `hrconnex.thm`
- `https://hrconnex.thm/onboarding/15400654060/j.garcia`

## Classification

**False Positive**

### Closure Rationale

The alert was classified as a False Positive because investigation confirmed that HRConnex is an authorized third-party HR partner used for employee onboarding. Internal correspondence validated the expected onboarding activity, while security analysis of the embedded URL returned a **CLEAN** status.

No evidence of credential harvesting, malware delivery, malicious redirection, or other phishing activity was identified.

## Escalation Decision

**No escalation required.**

The investigation established legitimate business activity with no evidence of compromise or malicious behaviour.

## Outcome

Alert **8814** was investigated and closed as a **False Positive**.

The case demonstrates the importance of correlating automated security alerts with SIEM telemetry, business context, internal communications, and reputation analysis before determining whether an alert represents genuine malicious activity.


## Case 8814 – Investigation Evidence

### Evidence 1 – Initial Alert / Email Review
![Case 8814 Initial Alert](images/Screenshot%202026-08-14%20095130.png)

### Evidence 2 – SIEM Investigation
![Case 8814 SIEM Investigation](images/Screenshot%202026-08-14%20100553.png)

### Evidence 3 – SIEM Correlation
![Case 8814 SIEM Correlation](images/Screenshot%202026-08-14%20110323.png)

### Evidence 4 – Internal Email Validation
![Case 8814 Internal Email Validation](images/Screenshot%202026-08-14%20110603.png)

### Evidence 5 – URL Security Analysis
![Case 8814 URL Analysis](images/Screenshot%202026-08-14%20111127.png)

### Evidence 6 – Incident Classification / Closure
![Case 8814 False Positive Classification](images/Screenshot%202026-08-14%20112058.png)
