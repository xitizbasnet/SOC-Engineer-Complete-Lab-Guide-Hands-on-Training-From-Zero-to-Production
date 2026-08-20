# Module 7: CrowdStrike Console & Task Management

## Falcon Platform — EDR, RTR & Threat Hunting

CrowdStrike Falcon is a cloud-native endpoint security platform offering EDR, NGAV, threat intelligence, and real-time response capabilities. It is widely deployed in enterprise SOCs as the primary EDR/XDR platform.

---

# Lab 7.1 — Falcon Console Navigation

## Objective

**Navigate the Falcon UI and understand key modules.**

---

## Step 1: Falcon Console Key Modules

The Falcon Console provides multiple modules for endpoint security, detection, threat intelligence, response, and investigation.

| Module                             | Function                                                          |
| ---------------------------------- | ----------------------------------------------------------------- |
| **Endpoint Security → Hosts**      | View all enrolled endpoints, sensor version, and last seen status |
| **Endpoint Security → Detections** | View all EDR detections with severity and MITRE mapping           |
| **Threat Intelligence → Intel**    | View actor profiles, IOC management, and intelligence reports     |
| **Response → Real Time Response**  | Provides remote shell access to endpoints                         |
| **Investigate → Falcon Insight**   | Provides process graphs, event search, and threat hunting         |
| **Dashboards**                     | Provides customisable SOC overview dashboards                     |

> 🧭 **Navigation Tip:** Become familiar with the relationship between endpoint inventory, detections, threat intelligence, investigation, and response workflows.

---

## Step 2: Host Management

### Download Sensor Packages

Navigate to:

**Hosts** → **Sensor Downloads**

Obtain the appropriate deployment package for:

* Windows
* Linux
* Mac

### Windows Deployment

Use:

```cmd
msiexec /i falcon-sensor.msi CID=<CID> /quiet
```

### Linux Deployment

Install the RPM package:

```bash
sudo rpm -ivh falcon-sensor-<version>.el8.x86_64.rpm
```

Configure the CrowdStrike Customer ID:

```bash
sudo /opt/CrowdStrike/falconctl -s --cid=<CID>
```

### Verify Host Enrollment

In the Falcon Console, navigate to:

**Hosts** → **All Hosts**

Filter by:

```text
Last Seen < 1 hour
```

> 🔎 **Validation:** Confirm that recently deployed endpoints appear in the console and are reporting current sensor activity.

---

## Step 3: Configure Prevention Policies

1. Navigate to:

   **Configuration** → **Prevention Policies** → **Create New Policy**

2. Configure the following protections.

### Machine Learning

Enable:

**Machine Learning — Moderate+**

For production environments.

> For high-risk systems, use **Aggressive** according to the organisation's testing and change-management requirements.

### Additional Prevention Controls

Enable:

* **Exploit Mitigation**
* **Ransomware Protection**
* **Suspicious Processes**
* **Custom IOA (Indicators of Attack) rules**

### Assign Policy to Host Groups

Assign policies according to operating system and host role, such as:

* **Windows Servers**
* **Windows Workstations**
* **Linux**

> 🛡️ **Policy Management:** Use host groups to apply appropriate prevention policies consistently across different endpoint populations.

---

## ⭐ Best Practice Tips

* **Deploy the Falcon sensor via SCCM/Intune/Ansible for large fleets** — never perform manual installation at scale.
* **Always test prevention policy changes on a pilot group (5–10% of hosts)** before fleet rollout.
* **Reduced Functionality Mode (RFM) sensors are not protected** — check for RFM sensors daily.
* **Enable kernel-level sensor mode on critical servers** for the deepest visibility.
* **Monitor sensor health via API** for automated alerting on stale or offline sensors.
