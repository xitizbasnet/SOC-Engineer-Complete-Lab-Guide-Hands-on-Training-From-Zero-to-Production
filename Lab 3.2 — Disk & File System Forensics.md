# Lab 3.2 — Disk & File System Forensics

## Objective

**Analyse disk images for deleted files, artefacts and timeline.**

---

## Step 1: Mount Image & Examine File System with Autopsy

1. Open **Autopsy**.

2. Create a new case:

   **New Case** → **Add Data Source** → **Disk Image**

3. Select the forensic image:

   * `.dd`
   * `.E01`

4. Select the following modules:

   * **File Type Identification**
   * **Hash Lookup**
   * **Keyword Search**
   * **Recent Activity**

5. Wait for the ingest process to complete.

6. Explore the directory tree and review:

   * **Deleted Files**
   * **Unallocated Space**
   * **`$Recycle.Bin`**
   * **System artifacts**

---

## Step 2: Recover Deleted Files with Sleuth Kit

### List All Files Including Deleted Files

```bash id="m5q3hf"
fls -r disk_image.dd > file_listing.txt
```

### Show Deleted Files Only

```bash id="36xg2b"
grep 'r/r \*' file_listing.txt
```

### Recover a File by Inode

```bash id="1tzz3k"
icat disk_image.dd > recovered_file.ext
```

### Perform Bulk Unallocated Recovery

```bash id="j3f1e2"
tsk_recover -e disk_image.dd /output/recovered/
```

> 💡 **Note:** Preserve recovered files separately from the original forensic image and maintain appropriate evidence documentation.

---

## Step 3: Windows Artefact Analysis

### Prefetch Files

Location:

```text
C:\Windows\Prefetch\
```

Use **PECmd.exe** to parse execution history:

```cmd id="lsp5b9"
PECmd.exe -d C:\Windows\Prefetch\ --csv prefetch_output.csv
```

### Jump Lists

Location:

```text
%APPDATA%\Microsoft\Windows\Recent\AutomaticDestinations\
```

### LNK Files

Location:

```text
C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\
```

### Shellbags

Review `NTUSER.DAT`:

```text
SOFTWARE\Microsoft\Windows\Shell\BagMRU
```

> **Purpose:** Shellbags can provide evidence of folder access.

### SRUM

Location:

```text
C:\Windows\System32\sru\SRUDB.dat
```

> **Purpose:** SRUM can provide information about network usage and application execution history.

---

## Step 4: Build Forensic Timeline

Create a body file using Sleuth Kit:

```bash id="l6l7d7"
fls -m / -r disk_image.dd > bodyfile.txt
```

Generate the forensic timeline:

```bash id="1cax9x"
mactime -b bodyfile.txt -d > timeline.csv
```

### Analyse the Timeline

1. Open `timeline.csv` in **Excel** or **LibreOffice**.

2. Filter the timeline around the relevant incident date and time.

3. Look for:

   * Files created or modified during the suspicious window.
   * New executables in `Temp`.

4. Correlate the timeline with **event logs** to build a comprehensive picture of activity.

---

## ⭐ Best Practice Tips

* **The MFT (`$MFT`) is the most important file system artefact on NTFS** — parse it with **MFTECmd**.
* **File timestamps can be modified (timestomping)** — always check `$STANDARD_INFORMATION` vs `$FILE_NAME`.
* **VSS (Volume Shadow Copies)** often contain pre-infection versions of files — check them.
* **Browser history, cookies and cache** are goldmines for insider threat investigations.
* **Always verify hashes after mounting/copying** to ensure no modification occurred.
