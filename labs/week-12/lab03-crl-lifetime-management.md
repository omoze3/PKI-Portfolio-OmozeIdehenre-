# Week 12 – Lab 03: CRL Lifetime Management

## Objective

The objective of this lab was to examine Certificate Revocation List (CRL) publication settings, modify the CRL validity period, publish a new CRL, and verify the resulting changes. This exercise demonstrates how revocation information is managed and how CRL lifetimes affect certificate validation.

---

## Environment

- Server: PKI-SRV01
- Role: Enterprise Issuing CA
- Operating System: Windows Server 2022
- Service: Active Directory Certificate Services (AD CS)

---

## Step 1 – Verify Online Responder Service

Verified that the Online Responder Service was running.

### Command

```powershell
Get-Service OCSPSvc
```

### Result

```text
Status   Name      DisplayName
------   ----      -----------
Running  OCSPSvc   Online Responder Service
```

The Online Responder Service was functioning properly.

---

## Step 2 – Review Existing CRL Configuration

Reviewed the current CRL publication intervals configured on the Issuing CA.

### Commands

```powershell
certutil -getreg CA\CRLPeriod
certutil -getreg CA\CRLPeriodUnits
certutil -getreg CA\CRLDeltaPeriod
certutil -getreg CA\CRLDeltaPeriodUnits
```

### Original Values

```text
CRLPeriod = Weeks
CRLPeriodUnits = 1

CRLDeltaPeriod = Days
CRLDeltaPeriodUnits = 1
```

These settings indicated that a base CRL was published every week and a Delta CRL was published daily.

---

## Step 3 – Modify CRL Lifetime Settings

Changed the CRL publication intervals to demonstrate the impact of extended CRL validity.

### Commands

```powershell
certutil -setreg CA\CRLPeriod Years
certutil -setreg CA\CRLPeriodUnits 99

certutil -setreg CA\CRLDeltaPeriod Years
certutil -setreg CA\CRLDeltaPeriodUnits 99
```

### Result

```text
CRLPeriod = Years
CRLPeriodUnits = 99

CRLDeltaPeriod = Years
CRLDeltaPeriodUnits = 99
```

The CA accepted the new configuration values successfully.

---

## Step 4 – Verify Updated Settings

Confirmed the registry changes.

### Commands

```powershell
certutil -getreg CA\CRLPeriod
certutil -getreg CA\CRLPeriodUnits
certutil -getreg CA\CRLDeltaPeriod
certutil -getreg CA\CRLDeltaPeriodUnits
```

### Verified Output

```text
CRLPeriod = Years
CRLPeriodUnits = 99

CRLDeltaPeriod = Years
CRLDeltaPeriodUnits = 99
```

The configuration changes were successfully applied.

---

## Step 5 – Publish a New CRL

Generated a new CRL using the updated settings.

### Command

```powershell
certutil -crl
```

### Result

```text
CertUtil: -CRL command completed successfully.
```

The Certificate Authority successfully generated and published a new CRL.

---

## Step 6 – Locate the Generated CRL

Verified the CRL files stored by the Certificate Authority.

### Command

```powershell
dir "C:\Windows\System32\CertSrv\CertEnroll"
```

### Result

```text
CVI Issuing CA 1+.crl
CVI Issuing CA 1.crl
PKI-SRV01.corp.cvilab.local_CVI Issuing CA 1.crt
```

The updated CRL file was successfully generated.

---

## Step 7 – Examine the CRL

Inspected the generated CRL to review its validity period and publication schedule.

### Command

```powershell
certutil -dump "C:\Windows\System32\CertSrv\CertEnroll\CVI Issuing CA 1.crl"
```

### Result

```text
Next CRL Publish:
Thursday, May 31, 2125 10:54:35 PM
```

The extended CRL lifetime was reflected in the Next CRL Publish value.

---

## Observations

The CRL publication schedule directly affects how long revocation information remains valid.

Advantages of longer CRL periods:

- Reduced publishing frequency
- Lower administrative overhead
- Fewer updates distributed to clients

Disadvantages of longer CRL periods:

- Revocation information becomes stale
- Increased risk of clients relying on outdated status information
- Reduced responsiveness to certificate compromise events

For production environments, organizations typically use shorter CRL publication intervals to ensure revocation data remains current.

---

## Conclusion

In this lab, I successfully reviewed, modified, and verified Certificate Revocation List publication settings on the Issuing CA. After extending both the Base CRL and Delta CRL validity periods to 99 years, I generated a new CRL and confirmed the resulting publication schedule. This exercise demonstrated how CRL configuration affects revocation management and reinforced the importance of balancing operational efficiency with security requirements.
