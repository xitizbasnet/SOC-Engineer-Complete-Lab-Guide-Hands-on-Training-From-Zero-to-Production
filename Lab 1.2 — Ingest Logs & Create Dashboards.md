# Lab 1.2 — Ingest Logs & Create Dashboards

## Objective

**Configure data inputs and build security dashboards.**

---

## Step 1: Add Windows Event Log Input via Forwarder

1. On the Windows host with the Universal Forwarder (UF) installed, navigate to:

   **Settings** → **Data Inputs** → **Windows Event Log**

2. Select the following event channels:

   * **Security**
   * **System**
   * **Application**

3. Add the following `inputs.conf` entry:

```ini
[WinEventLog://Security]
disabled = false
index = security_logs
```

---

## Step 2: Add Syslog Input (Linux/Network Devices)

1. In Splunk, navigate to:

   **Settings** → **Data Inputs** → **UDP**

2. Configure the UDP input with the following settings:

   | Setting         | Value           |
   | --------------- | --------------- |
   | **Port**        | `514`           |
   | **Source Type** | `syslog`        |
   | **Index**       | `security_logs` |

3. On the firewall or router, configure the syslog destination:

   * **Syslog Server IP:** `<Splunk Server IP>`
   * **Port:** `514`
   * **Facility:** `local7`

4. Monitor the Splunk daemon log for syslog activity:

```bash
sudo tail -f /opt/splunk/var/log/splunk/splunkd.log | grep syslog
```

---

## Step 3: Verify Data Ingestion

1. Open the **Search & Reporting** app.

2. Run the following search to verify that security logs are being ingested:

```spl
index=security_logs | head 20
```

3. Check the available source types:

```spl
index=security_logs | stats count by sourcetype
```

> ⚠️ **Troubleshooting:** If no data appears, verify the forwarder's `outputs.conf` configuration and ensure that firewall rules allow traffic on port `9997`.

---

## Step 4: Build Security Dashboard

1. Navigate to:

   **Dashboards** → **Create New Dashboard**

2. Set the dashboard title to:

```text
SOC Overview
```

3. Add a panel using the following search:

```spl
index=security_logs EventCode=4625 | stats count by src_ip | sort -count
```

4. Configure the visualization as:

   **Bar Chart — Failed Logins by Source IP**

5. Add another panel using:

```spl
index=security_logs | timechart span=1h count by sourcetype
```

6. Add another panel using:

```spl
index=security_logs EventCode=4720 | table _time, user, src_ip
```

**Description:** New Account Created

7. Save the dashboard and set **Auto-Refresh** to **5 minutes**.

---

## ⭐ Best Practice Tips

* **Use the `tstats` command** for fast searches over large data volumes (uses tsidx acceleration).
* **Always set a time-range picker** on dashboards to avoid accidental all-time scans.
* **Normalize all timestamps to UTC** at ingestion to avoid correlation issues.
* **Tag data with environment** (`prod`/`dev`) and business unit in `props.conf` for easier filtering.
