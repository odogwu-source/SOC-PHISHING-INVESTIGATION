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

