# Week 13 - Lab 01: Active Directory Certificate Services (AD CS) Backup

**Course:** Public Key Infrastructure (PKI)  
**Week:** 13  
**Lab:** 01 – Active Directory Certificate Services (AD CS) Backup
**Platform:** Windows Server 2022  
**Virtual Machine:** PKI-SRV01  
**Date Completed:** June 26, 2026

## Objective

Perform a full backup of an Enterprise Issuing Certificate Authority by exporting:

- CA private key and certificate
- Certificate Services database
- Transaction logs

Validate backup integrity and republish the Certificate Revocation List (CRL).

---

## Environment

| Item | Value |
|------|-------|
| Operating System | Windows Server 2022 |
| Role | Enterprise Issuing CA |
| CA Name | CVI Issuing CA 1 |
| Domain | corp.cvilab.local |
| Server | PKI-SRV01 |

---

## Verify CA Health

Verified Certificate Services was operational.

### Verify Certificate Service

```powershell
Get-Service CertSvc
```

Result:

- Status: Running

### Verify CA Connectivity

```powershell
certutil -ping
```

Result:

- Successfully connected to **CVI Issuing CA 1**
- ICertRequest2 interface responding

---

## Create Backup Directory

```powershell
New-Item -ItemType Directory -Path C:\CABackup -Force
```

Result:

- Backup directory created successfully.

---

## Perform CA Backup

Executed a full Certificate Services backup.

```powershell
certutil -backup -p <BackupPassword> C:\CABackup
```

The backup included:

- Private Key (.p12)
- CA Certificate
- Certificate Database (.edb)
- Transaction Logs

Result:

Backup completed successfully.

---

## Verify Backup Files

Verified backup contents.

```powershell
dir C:\CABackup
```

Verified:

- CVI Issuing CA 1.p12
- Database folder

Verified database contents.

```powershell
dir C:\CABackup\Database
```

Verified:

- CVI Issuing CA 1.edb
- certbkxp.dat
- edb00002.log
- edb00003.log

---

## Verify CA Availability

Confirmed the CA remained operational after backup.

```powershell
certutil -ping
```

Result:

- CA responded successfully.

---

## Publish New Certificate Revocation List

Republished the CRL.

```powershell
certutil -CRL
```

Result:

```
CertUtil: -CRL command completed successfully.
```

---

## System State Backup

The System State backup portion of the lab could not be completed.

### Reason

The CVI lab virtual machine contains only a single usable volume (C:).

Windows Server Backup (`wbadmin`) requires a destination volume separate from the operating system volume for System State backups.

Because no additional disk or volume was available, this step could not be performed.

---

## Skills Demonstrated

- Enterprise AD CS administration
- Certificate Authority backup
- Private key protection
- PKCS#12 export
- Certificate database backup
- Transaction log backup
- Backup verification
- Certificate Services health validation
- CRL publication
- Disaster recovery preparation

---

## Lessons Learned

Backing up an Enterprise Certificate Authority requires protecting both the CA private key and the Certificate Services database. These components are essential for restoring certificate issuance and maintaining trust relationships after a failure.

Although the lab environment prevented a System State backup because only a single system volume was available, all critical AD CS backup components were successfully created and verified.

This exercise reinforced the importance of validating backup integrity and ensuring the CA remains operational after backup operations.

---

## Lab Status

**Completed Successfully**

**Exception:**

System State backup was not possible due to the single-volume limitation of the lab environment.
