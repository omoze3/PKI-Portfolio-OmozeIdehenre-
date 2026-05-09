# Lab 01: Environment Verification & VM Connectivity Check

**Student Name:** Omoze Idehenre  
**Date Completed:** May 8, 2026  
**Phase:** 2 | **Week:** 9  

---

# Step 1 – VM Startup & Login

## VMs started in correct order (DC01 → PKI-SRV01 → Root-CA)

- [x] Yes
- [ ] No — describe what happened:

## Login Credentials Used

| VM | Account Used | Login Successful? |
|---|---|---|
| DC01 | CORP\pki.admin | Yes |
| PKI-SRV01 | CORP\pki.admin | Yes |
| Root-CA | Not Started | N/A |

## Notes / Issues Encountered

DC01 was started first and allowed time to fully load before starting PKI-SRV01. Root-CA remained powered off because it is an offline Root CA and was not required for this lab.

---

# Step 2 – VM Connectivity Test

## Command Run on PKI-SRV01

```powershell
Test-Connection -ComputerName DC01 -Count 2
```

## Output Received

```powershell
Source        Destination     IPV4Address     IPV6Address                               Bytes    Time(ms)
------        -----------     -----------     -----------                               -----    --------
PKI-SRV01     DC01            192.168.10.10   fd59:f2f3:25c7:3f2a:a5c1:4a7a:eb34:9553  32       2
PKI-SRV01     DC01            192.168.10.10   fd59:f2f3:25c7:3f2a:a5c1:4a7a:eb34:9553  32       4
```

## DC01 Responded Successfully

- [x] Yes
- [ ] No — troubleshooting steps taken:

---

# Step 3 – CertSvc Service Status

## Command Run on PKI-SRV01

```powershell
Get-Service CertSvc
```

## Output Received

```powershell
Status   Name      DisplayName
------   ----      -----------
Running  CertSvc   Active Directory Certificate Services
```

## CertSvc Status Shown

`Running`

## Service Was Running

- [x] Yes
- [ ] No — action taken:
---

# Step 4 – Certification Authority Console

## Steps Completed on PKI-SRV01

- [x] Opened `certsrv.msc` via Run dialog
- [x] Confirmed CA name visible: `CVI Issuing CA 1`
- [x] Confirmed CA status: `Running / Green Icon`
- [x] Expanded left pane — folders visible (Revoked Certificates, Issued Certificates, Pending Requests, Failed Requests, Certificate Templates)

## Screenshot or Description of certsrv.msc

The Certification Authority console opened successfully and displayed the `CVI Issuing CA 1` enterprise issuing CA with a green operational status icon. The left navigation pane displayed Revoked Certificates, Issued Certificates, Pending Requests, Failed Requests, and Certificate Templates, confirming that the CA service and console were functioning properly.
---

# Step 5 – Certificate Log File Path

## Command Run on PKI-SRV01

```powershell
Get-ChildItem "C:\Windows\System32\CertLog"
```

## Output Received

```powershell
Directory: C:\Windows\System32\CertLog

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         5/9/2026  7:14 AM         1048576 CVI Issuing CA 1.edb
-a----         5/9/2026  7:14 AM           16384 CVI Issuing CA 1.jfm
-a----         5/9/2026  7:14 AM            8192 edb.chk
-a----         5/9/2026  7:14 AM         1048576 edb.log
-a----         4/25/2026 7:45 PM         1048576 edbres00001.jrs
-a----         4/25/2026 7:45 PM         1048576 edbres00002.jrs
-a----         4/25/2026 7:45 PM         1048576 edbtmp.log
-a----         5/9/2026  7:14 AM           20480 tmp.edb
```

## Files Found in CertLog

| File Name | Approximate Size |
|---|---|
| CVI Issuing CA 1.edb | 1 MB |
| CVI Issuing CA 1.jfm | 16 KB |
| edb.chk | 8 KB |
| edb.log | 1 MB |
| edbres00001.jrs | 1 MB |
| edbres00002.jrs | 1 MB |
| edbtmp.log | 1 MB |
| tmp.edb | 20 KB |

---

# Reflection

## One Thing That Went Well During This Lab

The environment verification process completed successfully and confirmed that the PKI infrastructure was functioning correctly. Connectivity between PKI-SRV01 and DC01 worked properly, and the Certification Authority services were already running without requiring troubleshooting.

## One Thing That Was Confusing or Unexpected

One unexpected aspect was seeing how many database and transaction log files existed inside the CertLog directory. It helped reinforce that Active Directory Certificate Services relies on a structured database and logging system similar to other enterprise infrastructure services.

---

# Submission Checklist

- [x] All five steps completed
- [x] All command outputs pasted
- [x] Reflection section filled in
- [x] File saved as `lab-01-environment-verification.md`
- [x] File committed to portfolio repo under `labs/week-09/`
