# Week 12 – Lab 03: Simulate a CRL Publication Failure

## Student Information

Student Name: Omoze Idehenre 
Date Completed: 5/31/2026
Phase: 2 | Week: 12
Submission Path: labs/week-12/lab-03-crl-failure-simulation.md

---

# Objective

The objective of this lab was to simulate a Certificate Revocation List (CRL) publication failure by intentionally modifying CRL publication settings on the Issuing Certificate Authority. This exercise demonstrated how improper CRL configuration can impact revocation checking, certificate validation, and PKI operational reliability.

---

# Pre-Lab Verification

## Service Verification

Executed:

```powershell
Get-Service CertSvc
Get-Service OCSPSvc
```

Result:

```text
CertSvc  - Running
OCSPSvc  - Running
```

Both required services were operational.

---

# Record Existing CRL Configuration

Executed:

```powershell
certutil -getreg CA\CRLPeriod
certutil -getreg CA\CRLPeriodUnits
certutil -getreg CA\CRLDeltaPeriod
certutil -getreg CA\CRLDeltaPeriodUnits
```

Recorded Values:

| Setting             | Value |
| ------------------- | ----- |
| CRLPeriod           | Weeks |
| CRLPeriodUnits      | 1     |
| CRLDeltaPeriod      | Days  |
| CRLDeltaPeriodUnits | 1     |

---

# Part A – Create the CRL Publication Failure

## Modify CRL Publication Schedule

Executed:

```powershell
certutil -setreg CA\CRLPeriodUnits 99
certutil -setreg CA\CRLPeriod Years
certutil -setreg CA\CRLDeltaPeriodUnits 99
certutil -setreg CA\CRLDeltaPeriod Years
```

Result:

```text
CertUtil: -setreg command completed successfully.
```

for all four commands.

---

## Restart Certificate Services

Executed:

```powershell
net stop CertSvc
net start CertSvc
```

Result:

Certificate Services restarted successfully.

---

## Verify Modified Configuration

Executed:

```powershell
certutil -getreg CA\CRLPeriod
certutil -getreg CA\CRLPeriodUnits
certutil -getreg CA\CRLDeltaPeriod
certutil -getreg CA\CRLDeltaPeriodUnits
```

Verified Values:

```text
CRLPeriod = Years
CRLPeriodUnits = 99

CRLDeltaPeriod = Years
CRLDeltaPeriodUnits = 99
```

---

## Publish New CRL

Executed:

```powershell
certutil -CRL
```

Result:

```text
CertUtil: -CRL command completed successfully.
```

---

## Examine the Generated CRL

Executed:

```powershell
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\CVI Issuing CA 1.crl"
```

Observed:

```text
Next CRL Publish:
Thursday, May 31, 2125 10:54:35 PM
```

This confirmed that the CA would not attempt another scheduled CRL publication for approximately ninety-nine years.

---

# Part B – Observe the Failure

The simulated failure demonstrated how CRL publication schedules directly impact certificate revocation data.

The CA remained operational and continued issuing certificates; however, revocation information would eventually become stale because new CRLs would not be generated on a normal schedule.

If the CRL were allowed to expire without publication of a replacement CRL, relying parties could encounter revocation checking failures or continue trusting revoked certificates depending on application behavior.

---

# Part C – Diagnose the Failure

## Root Cause

The root cause of the simulated failure was the modification of the following Certificate Authority registry values:

```text
CRLPeriod = Years
CRLPeriodUnits = 99

CRLDeltaPeriod = Years
CRLDeltaPeriodUnits = 99
```

These values instructed the Certificate Authority to postpone future CRL publications for ninety-nine years.

Because revocation checking relies on fresh CRL data, extending publication intervals creates a condition where revocation information becomes outdated.

---

## Security Impact

Potential impacts include:

* Stale revocation information
* Increased risk of trusting revoked certificates
* Delayed detection of compromised certificates
* Certificate validation failures
* Reduced PKI reliability

---

# Part D – Restore Normal Operation

## Restore Original Configuration

Executed:

```powershell
certutil -setreg CA\CRLPeriodUnits 1
certutil -setreg CA\CRLPeriod Weeks
certutil -setreg CA\CRLDeltaPeriodUnits 1
certutil -setreg CA\CRLDeltaPeriod Days
```

---

## Restart Certificate Services

Executed:

```powershell
net stop CertSvc
net start CertSvc
```

---

## Publish Fresh CRL

Executed:

```powershell
certutil -CRL
```

Result:

```text
CertUtil: -CRL command completed successfully.
```

---

## Verify Restored Values

Expected:

```text
CRLPeriod = Weeks
CRLPeriodUnits = 1

CRLDeltaPeriod = Days
CRLDeltaPeriodUnits = 1
```

The original publication schedule was successfully restored.

---

# Part E – Incident Report

## Failure

A simulated CRL publication failure was created by modifying the Certificate Authority CRL publication schedule from weekly and daily intervals to ninety-nine years. The CA accepted the configuration and published a new CRL reflecting the extended publication schedule.

The resulting CRL indicated a future publication date in the year 2125, demonstrating how revocation information could become stale if publication schedules are misconfigured.

## Detection

The issue was identified through review of Certificate Authority registry settings and inspection of the generated CRL using CertUtil.

Verification of the generated CRL revealed an abnormal publication interval extending approximately ninety-nine years into the future.

## Resolution

The original CRL publication values were restored to their default configuration, Certificate Services was restarted, and a fresh CRL was published.

Verification confirmed that normal publication schedules were restored and the Certificate Authority returned to standard operation.

---

# Reflection Questions

## 1. How would you detect this issue in production?

I would implement monitoring that validates CRL expiration dates and publication schedules. Monitoring only event logs would not be sufficient because a misconfigured CRL publication interval may not generate an error while still creating stale revocation data.

## 2. What security risk does a stale CRL create?

A stale CRL may allow revoked certificates to continue being trusted. If a certificate is compromised and revocation information is not updated, relying parties may continue accepting that certificate.

## 3. Why is CRL publication important?

CRL publication ensures that certificate revocation information remains current. Without regular CRL updates, certificate validation systems cannot reliably determine whether certificates should still be trusted.

---

# Conclusion

In this lab, I successfully simulated a CRL publication failure by modifying Certificate Authority publication intervals from their default values to ninety-nine years. After publishing a new CRL and verifying the resulting publication schedule, I demonstrated how revocation information can become stale due to configuration errors. The original settings were restored, Certificate Services was restarted, and normal CRL publication operations were re-established.

