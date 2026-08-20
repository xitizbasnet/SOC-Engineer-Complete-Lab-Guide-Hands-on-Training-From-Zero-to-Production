# Module 1: SIEM

## Security Information & Event Management — Splunk Core

> 🔐 **Module:** 01
> **Focus:** Security Information & Event Management (SIEM)
> **Platform:** Splunk Core

SIEM (Security Information and Event Management) is the backbone of a SOC. It aggregates, normalises, and correlates logs from across the environment to surface threats, generate alerts, and drive investigations. Splunk is the industry-leading SIEM platform used in enterprise SOCs worldwide.

---

# Lab 1.1 — Deploy & Configure Splunk SIEM

## Objective

**Install Splunk Enterprise and perform initial hardening.**

---

## Step 1: Download & Install Splunk Enterprise

1. Navigate to **splunk.com** → **Free Trial** → **Download Splunk Enterprise** (`.deb` / `.rpm` / `.tgz`).

2. Install the Splunk Enterprise package:

```bash
sudo dpkg -i splunk--linux-2.6-amd64.deb
```

3. Start Splunk and accept the license:

```bash
sudo /opt/splunk/bin/splunk start --accept-license
```

4. Enable Splunk to start automatically at system boot:

```bash
sudo /opt/splunk/bin/splunk enable boot-start
```

5. Open a browser and navigate to:

```text
http://localhost:8000
```

6. Log in with:

```text
Username: admin
Password: (set password)
```

> ⚠️ **Security Note:** Set a strong administrative password during the initial installation and do not retain default credentials.

---

## Step 2: Configure Receiving Port (Indexer)

1. In Splunk Web, navigate to:

   **Settings** → **Forwarding and Receiving** → **Configure Receiving** → **New Receiving Port**

2. Configure the receiving port as:

```text
9997
```

> 💡 **Note:** Port **9997** is the default Splunk forwarder-to-indexer port.

Alternatively, enable the Splunk listening port from the command line:

```bash
sudo /opt/splunk/bin/splunk enable listen 9997 -auth admin:password
```

---

## Step 3: Install Universal Forwarder on Linux Agent

1. Download **Splunk Universal Forwarder** from **splunk.com**.

2. Install the Universal Forwarder package:

```bash
sudo dpkg -i splunkforwarder--linux-2.6-amd64.deb
```

3. Start the Universal Forwarder and accept the license:

```bash
sudo /opt/splunkforwarder/bin/splunk start --accept-license
```

4. Configure the forwarder to send data to the Splunk indexer on port `9997`:

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server :9997 -auth admin:password
```

5. Configure the forwarder to monitor `/var/log/syslog`:

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/syslog
```

6. Restart the Universal Forwarder:

```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

---

## Step 4: Configure `indexes.conf`

1. Open the local Splunk index configuration file:

```bash
sudo nano /opt/splunk/etc/system/local/indexes.conf
```

2. Add the following configuration:

```ini
[security_logs]
homePath = $SPLUNK_DB/security_logs/db
coldPath = $SPLUNK_DB/security_logs/colddb
thawedPath = $SPLUNK_DB/security_logs/thaweddb
```

3. Restart Splunk to apply the configuration:

```bash
sudo /opt/splunk/bin/splunk restart
```

---

## ⭐ Best Practice Tips

* **Always change the default admin password immediately after install.**
* **Run Splunk as a non-root user for production environments.**
* **Enable TLS/SSL on port `8089` (management) and `9997` (forwarder) using your CA cert.**
* **Set index retention (`maxTotalDataSizeMB`) per compliance requirements (e.g. 365 days).**
* **Use Deployment Server (DS) to manage forwarder configurations at scale.**

---


