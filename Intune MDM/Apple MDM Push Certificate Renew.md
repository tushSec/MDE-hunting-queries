# Apple MDM Push Certificate Renewal Process (Microsoft Intune)

## Overview

The Apple Push Notification Service (APNs) certificate enables communication between Microsoft Intune and Apple devices (iPhone, iPad, and macOS). This certificate must be renewed annually before expiration to maintain device management capabilities.

> **Important:** Always renew the APNs certificate using the same Apple ID that was used to create the original certificate. Using a different Apple ID may require devices to be re-enrolled.

---

## Prerequisites

### Required Permissions

* Microsoft Intune Administrator
* Global Administrator (recommended)
* Access to the original Apple ID used to create the APNs certificate

### Required Information

* Apple ID associated with the current APNs certificate
* Access to Microsoft Intune Admin Center
* Internet access to the Apple Push Certificates Portal

---

## Verify Certificate Expiration

1. Sign in to the Intune Admin Center.
2. Navigate to:

```text
Devices
 └─ iOS/iPadOS
     └─ iOS/iPadOS enrollment
         └─ Apple MDM Push Certificate
```

3. Review:

   * Expiration Date
   * Apple ID
   * Certificate Status

> Recommended renewal window: 30 days before expiration.

---

## Step 1 – Download the Certificate Signing Request (CSR)

1. In Intune, select:

```text
Devices
 └─ iOS/iPadOS
     └─ iOS/iPadOS enrollment
         └─ Apple MDM Push Certificate
```

2. Select **Download your CSR**.
3. Save the CSR file locally.

Example:

```text
IntuneCSR.csr
```

---

## Step 2 – Renew the APNs Certificate in Apple Portal

1. Browse to the Apple Push Certificates Portal.

2. Sign in using the original Apple ID.

3. Locate the existing certificate.

Verify:

* Same Apple ID
* Same MDM Vendor
* Correct expiration date

4. Select **Renew**.

5. Upload the CSR downloaded from Intune.

6. Complete the renewal process.

7. Download the renewed APNs certificate.

Example:

```text
MDM_ Microsoft Corporation_Certificate.pem
```

---

## Step 3 – Upload Renewed Certificate to Intune

1. Return to the Intune Admin Center.

2. Navigate to:

```text
Devices
 └─ iOS/iPadOS
     └─ iOS/iPadOS enrollment
         └─ Apple MDM Push Certificate
```

3. Select **Upload APNs Certificate**.

4. Upload the renewed certificate (.pem).

5. Select **Save**.

---

## Validation

### Verify Certificate Status

Confirm:

* Certificate status displays as Active
* Expiration date reflects the new validity period
* Apple ID matches the original registration account

### Device Validation

Test the following:

* Device check-in
* Policy deployment
* Application deployment
* Compliance evaluation
* Remote actions (Sync, Wipe, Retire)

---

## Troubleshooting

### APNSCertificateNotValid

Possible causes:

* Expired certificate
* Incorrect certificate uploaded
* Certificate generated from a different Apple ID
* Corrupted certificate file

Resolution:

1. Verify the certificate was renewed and not recreated.
2. Confirm the Apple ID matches the original enrollment account.
3. Re-upload the renewed certificate.
4. Allow time for synchronization.

---

### Devices Stop Checking In

Possible causes:

* APNs certificate expired
* Apple Push Notification communication failure

Resolution:

1. Verify certificate status in Intune.
2. Confirm APNs certificate expiration date.
3. Perform device sync testing.

---

### Wrong Apple ID Used

Impact:

* Existing enrolled Apple devices may lose management connectivity.
* Device re-enrollment may be required.

Prevention:

* Maintain documented ownership of the APNs Apple ID.
* Store recovery information securely.

---

## Best Practices

* Renew at least 30 days before expiration.
* Maintain a shared administrative mailbox for the Apple ID.
* Document the Apple ID owner and recovery information.
* Configure certificate expiration monitoring.
* Record renewal dates in operational documentation.
* Perform validation testing immediately after renewal.

---

## Rollback Considerations

APNs certificate renewals cannot be rolled back once a new certificate is uploaded.

If a new certificate is accidentally created instead of renewed:

* Existing Apple device management may break.
* Device re-enrollment may be required.

Always verify the renewal action before completing the process.

---

## References

* Microsoft Intune APNs Certificate Management
* Apple Push Certificates Portal Documentation
* Apple Device Enrollment and Management Documentation

---

## Change Record

| Version | Date       | Description               |
| ------- | ---------- | ------------------------- |
| 1.0     | YYYY-MM-DD | Initial document creation |
