# Module 3: Digital Forensics

## Evidence Acquisition, Disk Analysis, Network & Log Forensics

Digital Forensics involves the preservation, collection, analysis and reporting of digital evidence in a manner that is legally admissible. SOC analysts must follow strict procedures to maintain chain of custody.

---

# Lab 3.1 — Evidence Acquisition & Chain of Custody

## Objective

**Collect forensic images following legal and procedural standards.**

---

## Step 1: Prepare Forensic Workstation

1. Use a **dedicated forensic workstation** — never the suspect machine.

2. Write-block the evidence drive using a **hardware write blocker** or use software write protection:

### Linux

```bash
sudo hdparm -r1 /dev/sdb
```

> **Note:** This provides software write protection on Linux.

### Windows

```text
diskpart
→ select disk 1
→ attributes disk set readonly
```

3. Verify the write blocker:

   * Attempt a write operation.
   * Confirm that the write operation fails.

> 🚨 **Important:** A write blocker is essential to prevent accidental modification of digital evidence.

---

## Step 2: Acquire Disk Image with `dd`/`dcfldd`

### Using `dd`

Acquire the disk image with:

```bash
sudo dd if=/dev/sdb of=/evidence/disk_image.dd bs=512 conv=noerror,sync status=progress
```

### Using `dcfldd`

Acquire the disk image and generate a SHA-256 hash log:

```bash
sudo dcfldd if=/dev/sdb of=/evidence/disk_image.dd hash=sha256 hashlog=/evidence/hash.log
```

### Calculate Source and Image Hashes

Calculate the source device hash:

```bash
sha256sum /dev/sdb > /evidence/source_hash.txt
```

Calculate the forensic image hash:

```bash
sha256sum /evidence/disk_image.dd >> /evidence/source_hash.txt
```

> 🔐 **Integrity Check:** Compare both hashes — they must match to prove integrity.

---

## Step 3: Acquire Image with FTK Imager (Windows)

1. Open **FTK Imager**.

2. Navigate to:

   **File** → **Create Disk Image**

3. Select the source:

   **Physical Drive**

4. Select the destination format:

   **E01 (Expert Witness)**

   > The E01 format includes metadata and hash information.

5. Fill in the **Evidence Item Information**:

   * **Case #**
   * **Examiner**
   * **Date**
   * **Description**

6. Enable:

   **Verify images after they are created**

7. Save the forensic image and hash report.

8. Store the image and associated documentation in the **evidence repository**.

---

## Step 4: Complete Chain of Custody Form

Document the following information:

* **Evidence Item Number**
* **Description**
* **Date/Time Seized**
* **Seized By**
* **Hash values (MD5 + SHA256)**
* **Storage location**
* **Access log**

Each person who handles the evidence must:

* Sign the chain-of-custody form.
* Record the appropriate timestamp.

> ⚠️ **Legal / Procedural Warning:** Broken chain of custody can make evidence inadmissible in court.

---

## ⭐ Best Practice Tips

* **Order of Volatility:** Capture the most volatile data first — registers/cache → RAM → network state → disk.
* **Never boot the suspect machine** — use forensic boot media if live analysis is needed.
* **Always use a hardware write blocker** — software blockers can be circumvented or fail.
* **Keep duplicate images** — one for analysis and one locked in evidence storage.
* **Time synchronisation matters** — verify the system clock against NTP before acquisition.
