# Lab 3.3 — Network Forensics with Wireshark

## Objective

**Capture and analyse network traffic for evidence of intrusion.**

> ⚠️ **Authorisation Requirement:** Always obtain written authorisation before capturing network traffic.

---

## Step 1: Capture Network Traffic

### Capture Traffic from an Interface

Use `tcpdump` to capture traffic from the `eth0` interface:

```bash
sudo tcpdump -i eth0 -w /evidence/capture.pcap
```

### Capture Traffic from a Specific Host

To capture traffic associated with a specific host:

```bash
sudo tcpdump -i eth0 'host 192.168.1.100' -w /evidence/suspect_host.pcap
```

### Capture Using Wireshark

In Wireshark:

**Capture** → **Options** → **Select interface** → **Start**

> 🔐 **Evidence Handling:** Store captured PCAP files in the designated evidence repository and preserve their integrity during analysis.

---

## Step 2: Analyse PCAP in Wireshark

### Review Protocol Hierarchy

Navigate to:

**Statistics** → **Protocol Hierarchy**

> **Purpose:** Understand the composition of the captured network traffic.

### Review TCP Conversations

Navigate to:

**Statistics** → **Conversations** → **TCP**

> **Purpose:** Identify high-volume TCP connections that may warrant further investigation.

### Identify HTTP POST Requests

Use the following display filter:

```text
http.request.method == 'POST'
```

> **Investigation Focus:** Identify potential data exfiltration via HTTP POST.

### Identify Suspicious DNS Activity

Use:

```text
dns.qry.name contains 'evil' OR dns.resp.ttl < 60
```

> **Investigation Focus:** Identify potential DGA or fast-flux DNS activity.

### Identify SYN Scanning

Use:

```text
tcp.flags.syn==1 && tcp.flags.ack==0
```

> **Investigation Focus:** Identify potential SYN scans or port scans.

---

## Step 3: Extract Files from PCAP

### Extract HTTP Objects

In Wireshark:

**File** → **Export Objects** → **HTTP** → **Save All**

> **Purpose:** Extract files transferred over HTTP.

### Extract SMB Objects

In Wireshark:

**File** → **Export Objects** → **SMB**

> **Purpose:** Extract files transferred over SMB.

### Extract HTTP Objects with `tshark`

```bash
tshark -r capture.pcap --export-objects http,/output/http_objects/
```

### Analyse Extracted Files

Run extracted files through:

* **VirusTotal**
* **Malware analysis workflow**

> ⚠️ **Safety:** Treat extracted files as potentially malicious. Follow the established malware-analysis workflow before executing or opening suspicious files.

---

## Step 4: Identify C2 Beaconing

### Analyse Packet Intervals

In Wireshark, navigate to:

**Statistics** → **IO Graphs**

Plot packet intervals and look for regular beaconing patterns.

> **Indicator:** Regular beaconing may appear as flat, periodic spikes.

### Analyse Packet Timing and Destinations

Use:

```bash
tshark -r capture.pcap -T fields -e frame.time -e ip.dst -e tcp.dstport | sort
```

### Look for C2 Indicators

Investigate:

* Regular intervals, such as every **60 seconds**
* Small, constant packet sizes
* Unusual destination ports

### Resolve IP Addresses

**CMD:**

```cmd
tshark -r capture.pcap -q -z ip_hosts,tree
```

### Threat Intelligence Checks

Check identified IP addresses against:

* **AbuseIPDB**
* **Shodan**
* **VirusTotal**

---

## ⭐ Best Practice Tips

* **Always capture the full packet (`snaplen 65535`)** — truncated packets can lose critical evidence.
* **Use NetworkMiner** for automated extraction of transferred files from PCAP.
* **Look for protocol anomalies**, such as:

  * HTTP on port `443`
  * DNS over TCP
  * ICMP with large payloads (tunnelling)
* **For encrypted traffic analysis**, focus on **JA3/JA3S TLS fingerprints** for C2 identification.
* **Retain PCAPs for at least 90 days** (or according to the applicable compliance requirement) for post-incident analysis.
