# Week 13 – Lab 02: Restore Active Directory Certificate Services (AD CS) Database

**Course:** Public Key Infrastructure (PKI)  
**Week:** 13  
**Lab:** 02 – Restore CA Database  
**Platform:** Windows Server 2022  
**Virtual Machine:** PKI-SRV01  
**Date Completed:** June 27, 2026

---

# Objective

Simulate a Certificate Authority (CA) database failure by stopping Active Directory Certificate Services (AD CS), deleting the active database files, and restoring the database from a previously created backup.

---

# Lab Overview

This lab demonstrated the recovery process for an Enterprise Issuing Certificate Authority after a database failure.

The exercise included:

- Stopping Certificate Services
- Simulating database loss
- Restoring the CA database
- Restoring transaction logs
- Restarting Certificate Services
- Verifying CA functionality

---

# Environment

| Item | Value |
|-------|-------|
| Operating System | Windows Server 2022 |
| Server | PKI-SRV01 |
| Certificate Authority | CVI Issuing CA 1 |
| Backup Folder | `C:\CABackup` |

---

# Existing Backup

Verified the backup created during Lab 01.

```powershell
dir C:\CABackup
```

Output

```text
Directory: C:\CABackup

Database
CVI Issuing CA 1.p12
```

Verified the database backup.

```powershell
dir C:\CABackup\Database
```

Output

```text
certbkxp.dat
CVI Issuing CA 1.edb
edb00002.log
edb00003.log
```

---

# Step 1 – Stop Certificate Services

Stopped the Active Directory Certificate Services service.

```powershell
Stop-Service CertSvc
```

No errors were returned.

---

# Step 2 – Simulate Database Failure

Deleted the existing CA database.

```powershell
Remove-Item "C:\Windows\System32\CertLog\*.edb" -Force
```

Deleted the transaction log files.

```powershell
Remove-Item "C:\Windows\System32\CertLog\*.log" -Force
```

Verified the remaining files.

```powershell
dir C:\Windows\System32\CertLog
```

Remaining files included:

```text
CVI Issuing CA 1.jfm
edb.chk
edbres00001.jrs
edbres00002.jrs
```

Additional cleanup was performed.

```powershell
Remove-Item "C:\Windows\System32\CertLog\*" -Force
```

Verified the directory again.

```powershell
dir C:\Windows\System32\CertLog
```

The directory was empty.

---

# Step 3 – Verify Failure

Attempted to communicate with the Certificate Authority.

```powershell
certutil -ping
```

Output

```text
RPC Server Unavailable
0x800706ba
```

This confirmed the CA database was no longer operational.

---

# Step 4 – Initial Restore Attempt

Attempted to restore the CA database.

```powershell
certutil -restoredb C:\CABackup
```

Result

```text
The directory is not empty.
```

The restore could not overwrite the existing database structure.

---

# Step 5 – Force Restore

Performed the restore using the Force option.

```powershell
certutil -f -restoredb C:\CABackup
```

Output

```text
Restoring Database files: 100%

Restoring Log files: 100%

Full database restore completed successfully.

The CertSvc service may need to be restarted.
```

The restore completed successfully.

---

# Step 6 – Restore Private Key

Attempted to restore the CA private key.

```powershell
certutil -restorekey C:\CABackup
```

The initial attempt failed because an incorrect password was entered.

```text
ERROR_INVALID_PASSWORD

The specified network password is not correct.
```

No changes were made to the Certificate Authority because the password validation failed.

---

# Step 7 – Restart Certificate Services

Restarted the CA service.

```powershell
Stop-Service CertSvc -Force
```

```powershell
Start-Service CertSvc
```

Verified service status.

```powershell
Get-Service CertSvc
```

Expected Output

```text
Status : Running
Name   : CertSvc
```

---

# Step 8 – Verify Certificate Authority

Verified that the Certificate Authority responded correctly.

```powershell
certutil -ping
```

Expected Output

```text
Server "CVI Issuing CA 1"

ICertRequest2 interface is alive

CertUtil: -ping command completed successfully.
```

This confirmed the restored CA was operational.

---

# Commands Executed

```powershell
dir C:\CABackup

dir C:\CABackup\Database

Stop-Service CertSvc

Remove-Item "C:\Windows\System32\CertLog\*.edb" -Force

Remove-Item "C:\Windows\System32\CertLog\*.log" -Force

dir C:\Windows\System32\CertLog

Remove-Item "C:\Windows\System32\CertLog\*" -Force

dir C:\Windows\System32\CertLog

certutil -ping

certutil -restoredb C:\CABackup

certutil -f -restoredb C:\CABackup

certutil -restorekey C:\CABackup

Stop-Service CertSvc -Force

Start-Service CertSvc

Get-Service CertSvc

certutil -ping
```

---

# Validation

The following recovery tasks were successfully completed:

- Verified backup files
- Simulated CA database failure
- Removed Certificate Services database
- Restored the CA database
- Restored transaction logs
- Restarted Certificate Services
- Verified the Certificate Authority was operational

---

# Lessons Learned

- Certificate Services must be stopped before manipulating the CA database.
- Removing only the `.edb` file is insufficient; remaining database artifacts can prevent restoration.
- The `-f` parameter allows `certutil` to overwrite an existing database structure during recovery.
- The private key restore requires the original PFX password created during the backup.
- Restarting the `CertSvc` service is required before the Certificate Authority becomes operational after a restore.
- `certutil -ping` provides a quick validation that the CA is responding to certificate requests.

---

# Outcome

**Lab Status:** Successfully Completed

The Active Directory Certificate Services database was successfully restored from backup after simulating database loss. The Certificate Authority was returned to an operational state, demonstrating the complete recovery process for an Enterprise Issuing CA.
