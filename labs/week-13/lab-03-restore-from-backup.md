# Week 13 - Lab 03: Restore Active Directory Certificate Services from Backup

**Course:** Public Key Infrastructure (PKI)  
**Week:** 13  
**Lab:** 03 – Restore from Backup Files  
**Platform:** Windows Server 2022  
**Virtual Machine:** PKI-SRV01  
**Date Completed:** June 28, 2026

## Objective

Restore an Active Directory Certificate Services (AD CS) Certification Authority from a previously created backup by restoring both the CA database and the private key after simulating a catastrophic failure.

This lab demonstrates disaster recovery procedures required to return an Enterprise Issuing CA to an operational state.

---

# Lab Environment

| Item | Value |
|------|-------|
| Operating System | Windows Server 2022 |
| CA Server | PKI-SRV01 |
| Domain | corp.cvilab.local |
| Certification Authority | CVI Issuing CA 1 |
| Backup Location | C:\CABackup |
| Database Backup | C:\CABackup\Database |
| Backup Utility | CertUtil |
| Administrative Tool | Certification Authority (certsrv.msc) |

---

# Learning Objectives

By the end of this lab I was able to:

- Simulate an Active Directory Certificate Services failure
- Restore the CA database from backup
- Restore the CA private key (.P12)
- Restart Active Directory Certificate Services
- Validate CA functionality using CertUtil
- Confirm Certification Authority availability through the MMC console
- Troubleshoot database and RPC connectivity errors

---

# Prerequisites

Completed:

- Lab 01 – Backup Certification Authority
- Lab 02 – Recovery Simulation

Verified backup contents:

```
C:\CABackup
│
├── Database
│   ├── certbkxp.dat
│   ├── CVI Issuing CA 1.edb
│   ├── edb00002.log
│   └── edb00003.log
│
└── CVI Issuing CA 1.p12
```

---

# Scenario

A catastrophic failure occurred on the Enterprise Issuing Certification Authority.

The following components were lost:

- Certificate database
- Certificate logs
- CA private key
- CA certificate

Recovery was performed using the previously created backup.

---

# Step 1 – Verify Backup

Verified the backup files existed before beginning recovery.

```powershell
Get-ChildItem C:\CABackup -Recurse |
Select-Object FullName,Length,LastWriteTime
```

Verified:

- Database folder
- certbkxp.dat
- EDB database
- Transaction logs
- CA P12 certificate

---

# Step 2 – Stop Certificate Services

Stopped Active Directory Certificate Services.

```powershell
Stop-Service CertSvc
```

Verified:

```powershell
Get-Service CertSvc
```

Result:

```
Status : Stopped
```

---

# Step 3 – Remove Existing Database

Removed the existing database files.

```powershell
Remove-Item "C:\Windows\System32\CertLog\*" -Force
```

Verified directory contents.

```powershell
dir C:\Windows\System32\CertLog
```

---

# Step 4 – Restore the CA Database

Initial restore attempt:

```powershell
certutil -restoredb C:\CABackup
```

Received:

```
Directory is not empty.
```

After cleaning the CertLog directory, forced the restore.

```powershell
certutil -f -restoredb C:\CABackup
```

Result:

```
Restoring Database files: 100%

Restoring Log files: 100%

Full database restore completed successfully.
```

---

# Step 5 – Restore the Private Key

The CA private key had previously been deleted from:

```
certlm.msc

Certificates (Local Computer)

Personal

Certificates

CVI Issuing CA 1
```

Initial attempts failed due to an incorrect password.

Used the correct password.

```powershell
certutil -f -restorekey -p "PKIBackup!2026" C:\CABackup
```

Result:

```
Restored keys and certificates

RestoreKey command completed successfully.
```

---

# Step 6 – Restart Certificate Services

Restarted Active Directory Certificate Services.

```powershell
Restart-Service CertSvc
```

Verified:

```powershell
Get-Service CertSvc
```

Result:

```
Running
```

---

# Step 7 – Validate the Certification Authority

Verified CA communication.

```powershell
certutil -ping
```

Result:

```
Server "CVI Issuing CA 1"

ICertRequest2 interface is alive

CertUtil: -ping command completed successfully.
```

---

# Step 8 – Verify in Certification Authority Console

Opened:

```
certsrv.msc
```

Confirmed:

- CVI Issuing CA 1 displayed
- Green status icon
- Certification Authority operational

---

# Validation

## Service Status

```powershell
Get-Service CertSvc
```

Result:

```
Running
```

---

## Database Restore

```powershell
certutil -restoredb
```

Completed successfully.

---

## Private Key Restore

```powershell
certutil -restorekey
```

Completed successfully.

---

## RPC Validation

```powershell
certutil -ping
```

Result:

```
ICertRequest2 interface is alive.
```

---

## Certification Authority

Opened:

```
certsrv.msc
```

Confirmed:

- CA online
- Green status indicator
- Management console accessible

---

# Troubleshooting

## Error

```
Directory is not empty.
```

Resolution:

Removed the existing CertLog database files before restoring.

---

## Error

```
The specified network password is not correct.
```

Resolution:

Verified the correct password used during Lab 01.

Correct password:

```
PKIBackup!2026
```

---

## Error

```
RPC Server Unavailable
```

Resolution:

Restarted Certificate Services and waited for the CA to register its RPC interface before re-running:

```powershell
certutil -ping
```

---

## Error

```
RestoreKey command failed
```

Resolution:

Specified the correct backup location and password.

---

# Commands Used

```powershell
Stop-Service CertSvc

Get-Service CertSvc

Remove-Item "C:\Windows\System32\CertLog\*" -Force

certutil -f -restoredb C:\CABackup

certutil -f -restorekey -p "PKIBackup!2026" C:\CABackup

Restart-Service CertSvc

Get-Service CertSvc

certutil -ping

certsrv.msc
```

---

# Skills Demonstrated

- Active Directory Certificate Services (AD CS)
- Certification Authority Disaster Recovery
- Enterprise PKI Administration
- CA Database Recovery
- Certificate Private Key Recovery
- Windows Server Administration
- PowerShell
- CertUtil
- Microsoft PKI
- Disaster Recovery Validation
- Troubleshooting PKI Services
- RPC Connectivity Validation

---

# Key Takeaways

This lab demonstrated the complete recovery process for an Enterprise Issuing Certification Authority following a simulated failure. Both the Certification Authority database and private key were restored from backup, Certificate Services were restarted, and successful validation confirmed that the CA was once again capable of processing certificate requests.

The exercise reinforced the importance of maintaining secure backups of both the CA database and private keys, understanding the sequence required for a successful restoration, and validating service health through both command-line utilities and the Certification Authority management console. This process mirrors real-world PKI disaster recovery procedures used in enterprise environments to minimize downtime and preserve trust infrastructure.

---

# Lab Status

**Status:** Completed Successfully

**Outcome:**

- CA database restored
- Private key restored
- Certificate Services running
- RPC interface operational
- Certification Authority console accessible
- Enterprise Issuing CA fully recovered from backup
