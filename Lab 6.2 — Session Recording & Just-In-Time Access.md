# Lab 6.2 — Session Recording & Just-In-Time Access

## Objective

**Enable PSM session proxy and configure JIT privilege elevation.**

---

## Step 1: Configure PSM for RDP Session Recording

1. In **PVWA**, navigate to:

   **Administration** → **Platform Management** → **WindowsServerLocalAdministrator**

2. Enable the following settings:

   ```text
   RequirePrivilegedSessionMonitoringAndIsolation = Yes
   RecordAndSaveSessionActivity = Yes
   ```

3. Configure the PSM server to act as the jump server:

   ```text
   User → PSM → Target
   ```

   > 🛡️ **Access Requirement:** Users must connect through PSM and must never connect directly to the target using RDP.

4. Sessions are recorded as video (**DVR format**) and stored in the Vault.

---

## Step 2: Initiate PSM-Protected RDP Session

1. In **PVWA**, navigate to:

   **Accounts** → **Find `10.1.1.50` Administrator** → **Connect** → **Connect with PSM**

2. The browser launches the RDP session through PSM.

> 🔐 **Credential Protection:** Credentials are not exposed to the user.

3. The session is recorded from the start.

   > Recording cannot be disabled by the user.

4. Within the session:

   * All keystrokes are logged and searchable.
   * CMD activity is captured as part of the session record.

5. After the session, access the recording through:

   **PVWA** → **Reports** → **Privileged Session Monitoring**

---

## Step 3: Configure Just-In-Time Access

### JIT Access Model

JIT grants temporary access only when needed — there is **no standing privilege**.

1. In PVWA, navigate to:

   **Administration** → **Access Control** → **Configure JIT Settings**

2. Configure:

   | Setting                            | Value     |
   | ---------------------------------- | --------- |
   | **Maximum Access Duration**        | `4 hours` |
   | **Require Business Justification** | `Yes`     |

3. User requests access.

4. Manager approves the request in PVWA.

5. Access is granted for the limited approved window.

6. After the access window expires:

   * The account is automatically deprovisioned.
   * The password is rotated.

> 🕒 **JIT Principle:** Privileged access should exist only for the duration required to complete the authorised task.

---

## Step 4: Review Session Recordings for Audit

1. Navigate to:

   **PVWA** → **Reports** → **Privileged Session Reports**

2. Search using:

   * User
   * Date
   * Target

3. Select the relevant session.

4. Select:

   **Play Recording**

5. Review the actions taken during the session.

6. Export the session log as text for **SIEM ingestion**.

### Flag Suspicious Activity

Flag sessions containing:

* Unusual commands
* Large file transfers
* Registry edits

### Integrate with SIEM

Forward **CyberArk Vault audit logs** to the SIEM using **syslog**.

> 🔎 **Audit Objective:** Session monitoring should provide a searchable record of privileged activity and support security investigations, compliance reviews, and incident response.

---

## ⭐ Best Practice Tips

* **Implement session recording for ALL privileged sessions** — including IT, vendors, and contractors.
* **Store session recordings in the Vault (encrypted)** — not on file shares.
* **Set up real-time alerts for high-risk commands** within PSM, including:

  * `net user /add`
  * `reg import`
  * `bcdedit`
* **Vendor access is higher risk** — always require JIT, dual approval, and session recording.
* **Conduct regular random audits of privileged sessions** — these act as a powerful deterrent against misuse.
