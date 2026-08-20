# Lab 1.3 — Write Correlation Rules & Alerts

## Objective

**Build SPL-based detection logic and configure alerting.**

---

## Step 1: Create Brute-Force Detection Rule

1. Create a new search using the following SPL:

```spl
index=security_logs EventCode=4625
| bucket _time span=5m
| stats count by src_ip, _time
| where count>10
```

2. Select:

   **Save As** → **Alert**

3. Configure the alert:

   | Setting               | Configuration                      |
   | --------------------- | ---------------------------------- |
   | **Title**             | `Brute Force Detected`             |
   | **Trigger Condition** | Number of Results > 0              |
   | **Throttle**          | Once every 10 minutes per `src_ip` |

---

## Step 2: Create Lateral Movement Detection

Use the following SPL search:

```spl
index=security_logs EventCode=4624 Logon_Type=3
| stats dc(ComputerName) as hosts by user
| where hosts>5
```

* Alert on a user authenticating to more than **5 hosts** in the time window.
* Add to the correlation search: save with **CIM-compliant field mapping**.

---

## Step 3: Configure Alert Actions

Configure the required alert actions:

### 📧 Send Email

* **Alert Actions** → **Send Email**
* **Recipient:** `soc-team@company.com`

### 🎫 Run Script

* **Alert Actions** → **Run Script**
* **Script:**

```text
/opt/splunk/bin/scripts/create_ticket.sh
```

### 🔗 Webhook

* **Alert Actions** → **Webhook**
* **URL:**

```text
http://jira.company.com/api/ticket
```

> 💡 **Purpose:** Use the webhook for automated ticket creation.

> ⚠️ **Testing:** Test the webhook with `curl` before saving the alert.

---

## Step 4: Tune & Suppress False Positives

1. Review triggered alerts for known-good IP addresses, such as **IT administrator jump servers**.

2. Add a lookup table:

   **Settings** → **Lookups** → **Lookup Table Files** → `whitelist_ips.csv`

3. Use the following search to identify events associated with the whitelist:

```spl
index=security_logs EventCode=4625
[ inputlookup whitelist_ips.csv
| rename ip as src_ip
| table src_ip ]
| stats count
```

4. Exclude whitelisted IPs from alert searches using:

```spl
NOT [ inputlookup ... ]
```

> 💡 **Tuning Guidance:** Maintain documented and justified exceptions for known-good systems and administrators to reduce false positives without weakening detection coverage.

---

## ⭐ Best Practice Tips

* **Use Splunk ESCU (Enterprise Security Content Update) app** for pre-built detections mapped to **MITRE ATT&CK**.
* **Always throttle alerts** — unbounded alerts cause alert fatigue and can cause analysts to miss real incidents.
* **Use risk-based alerting (RBA):** assign risk scores per event and alert when the risk score exceeds a defined threshold.
* **Document every suppression decision** in a tuning log with justification and review date.
* **Test correlation rules against historical data** before going live.
