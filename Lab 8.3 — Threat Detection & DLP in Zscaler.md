# Lab 8.3 — Threat Detection & DLP in Zscaler

## Objective

**Configure inline threat prevention and data loss prevention.**

---

## Step 1: Enable Advanced Threat Protection

1. In ZIA, navigate to:

   **Policy** → **Advanced Threat Protection**

2. Enable the following protections:

   * **Malware Protection**
   * **Ransomware Protection**
   * **Botnet Control**

### Enable AI-Powered Sandboxing

Enable **AI-Powered Sandboxing (Zscaler Sandbox)** for all file downloads.

Configure:

```text
Unknown files → Hold and analyse
```

> 🛡️ **Security Requirement:** Unknown files should be held and analysed rather than simply detected after download.

### Enable Phishing Detection

Enable **Phishing Detection** for all:

* HTTP traffic
* HTTPS traffic

---

## Step 2: Configure DLP Rules

1. Navigate to:

   **Policy** → **Data Loss Prevention** → **Add DLP Rule**

### Configure PCI Detection

Set the data type to:

```text
PCI (Credit Card Numbers)
```

Use the built-in detection engine.

### Configure DLP Channels

Apply the policy to:

* **HTTP Upload**
* **Cloud Email**
* **WebEx**
* **Slack**

### Configure the Action

Set:

```text
Action: Block + Alert + Notify Manager
```

### Additional DLP Rules

Create additional rules for:

* **PII**

  * Aadhaar numbers for India
  * PAN numbers for India
* **Source Code**
* **Confidential documents**

### DLP Fingerprinting

Use **DLP fingerprinting** for proprietary documents using **exact data matching**.

> 🔐 **Data Protection Principle:** Use content-aware controls to prevent sensitive and proprietary information from leaving approved channels.

---

## Step 3: Review Security Reports & Analytics

### Review Threat Activity

Navigate to:

**Insights** → **Reports** → **Threat Activity**

Review the top blocked threats by:

* User
* Location

### Investigate DLP Violations

Navigate to:

**Insights** → **Reports** → **DLP Violations**

Investigate identified policy violations.

### Export Activity Logs to Splunk

Navigate to:

**Insights** → **Activity Logs**

Export activity data to Splunk through **NSS** for SIEM correlation.

### Configure Scheduled Reports

Create scheduled reports for:

* **Weekly:** DLP summary
* **Monthly:** Threat trends

Send the reports to the **CISO**.

### Advanced Investigation

Use **ZIA Workbench** for advanced, query-based investigation of user traffic logs.

> 🔎 **Investigation Objective:** Combine threat activity, DLP violations, and user traffic telemetry to identify suspicious behaviour and data-loss events.

---

## ⭐ Best Practice Tips

* **Sandbox unknown executables AND Office macros** — not just `.exe` files.
* **DLP false positives erode trust** — tune rules using a minimum **2 weeks of monitor-only mode** before enforcement.
* **Zscaler NSS logs contain full URL and content inspection data** — this information is invaluable for forensic investigations.
* **Integrate Zscaler with your EDR** — such as CrowdStrike or Defender — via API for unified threat context.
* **Test DLP rules with synthetic test data** — do not use real PII when validating policies before enforcement mode.
