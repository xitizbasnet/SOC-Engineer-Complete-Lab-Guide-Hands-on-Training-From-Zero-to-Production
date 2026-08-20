# Lab 4.2 — Threat Intelligence Integration

## Objective

**Operationalise threat intel feeds into SOC workflows.**

---

## Step 1: Ingest OSINT Threat Feeds

### Splunk Threat Intelligence

1. In Splunk, install the **TA-Threat-Intel** app.
2. Configure the **AlienVault OTX API key**.

### MISP

1. Install **MISP (open source TIP)**.

2. Import feeds from:

   * **CIRCL**
   * **abuse.ch**
   * **EmergingThreats**

3. List the available MISP intelligence modules:

```bash id="82l36p"
misp-modules -l
```

### Shodan

Integrate the **Shodan API** to obtain asset exposure intelligence.

### Threat Intelligence Subscriptions

Subscribe to:

* **CISA advisories**
* **MS-ISAC**
* **Sector-specific ISACs**

---

## Step 2: Create IOC Lookup Tables in Splunk

### Download Malicious IP List

Use the following command to download the malicious IP list:

```cmd id="n9q7s2"
curl https://feodotracker.abuse.ch/downloads/ipblocklist.csv -o iocs.csv
```

### Upload the Lookup Table

In Splunk, navigate to:

**Settings** → **Lookups** → **Lookup Table Files**

Upload:

```text
iocs.csv
```

### Create Lookup Definition

Create the lookup definition:

```text
bad_ips.csv
```

Configure the following fields:

| Field             | Description      |
| ----------------- | ---------------- |
| `ip`              | IP address       |
| `threat_category` | Threat category  |
| `confidence`      | Confidence level |

### Query Network Logs Against the IOC Lookup

Use:

```spl id="y5d4jk"
index=network_logs
| lookup bad_ips src_ip as ip OUTPUT threat_category
| where isnotnull(threat_category)
```

### Configure Automatic Refresh

Set the lookup to automatically refresh:

**Every 24 hours**

> 💡 **Tip:** Automating IOC enrichment allows analysts to receive threat context during investigation without manually querying individual intelligence sources.

---

## Step 3: MITRE ATT&CK Mapping

Use **MITRE ATT&CK Navigator**:

https://mitre-attack.github.io/attack-navigator/

### Detection Coverage

* Map every current detection to the appropriate **ATT&CK Tactic/Technique**.
* Identify coverage gaps.
* Prioritise new detection rules for uncovered techniques.
* Tag all incidents with **ATT&CK TTP codes** for trend analysis.
* Share the **ATT&CK coverage map** with management monthly.

> 🎯 **Objective:** Use ATT&CK mapping to identify detection gaps and guide the SOC's detection engineering priorities.

---

## ⭐ Best Practice Tips

* **Enrich every alert automatically with threat intel before analyst review** — reduces triage time by 40–60%.
* **Prioritise high-confidence, low-false-positive IOCs** — quantity ≠ quality in threat intel.
* **Establish an IOC lifecycle:** `ingest → validate → alert → expire` — IOCs older than 90 days lose relevance.
* **Participate in threat intel sharing communities** (ISACs, FS-ISAC, H-ISAC) relevant to your industry.
* **Validate threat intel against your environment before alerting** — a "global bad IP" may be your CDN.
