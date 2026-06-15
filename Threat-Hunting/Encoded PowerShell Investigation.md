# Encoded PowerShell Investigation – Benign PRTG Windows Update Check

## Overview

This investigation identifies PowerShell processes executed with Base64-encoded commands (`-EncodedCommand` or `-enc`).

During analysis, the activity was determined to be **benign** and associated with a **PRTG Windows Update monitoring/check mechanism**.

## Detection Query

```kusto
DeviceProcessEvents
| where FileName in~ ("powershell.exe", "pwsh.exe")
| where ProcessCommandLine contains "-enc"
    or ProcessCommandLine contains "-encodedcommand"
| project Timestamp,
          DeviceName,
          InitiatingProcessAccountName,
          ProcessCommandLine
| order by Timestamp desc
```

## Investigation Steps

1. Identify PowerShell executions using encoded commands.
2. Review the parent process and command-line arguments.
3. Decode the Base64 payload.
4. Validate the decoded script functionality.
5. Verify whether the execution originated from a legitimate monitoring or management solution.
6. Confirm expected behavior with the system owner or application team.

## Findings

- Encoded PowerShell execution was observed on the monitored endpoint.
- Decoding the command revealed functionality related to Windows Update status collection.
- The execution was initiated by the PRTG monitoring solution.
- No malicious indicators, persistence mechanisms, or post-exploitation activity were identified.
- Activity aligns with normal PRTG Windows Update monitoring operations.

## Classification

| Category | Value |
|-----------|---------|
| Alert Type | Encoded PowerShell Execution |
| Source | PRTG Monitoring |
| Impact | None |
| Severity | Informational |
| Verdict | Benign / Expected Activity |

## Recommendation

No remediation is required.

Consider creating an allow-list or suppression rule for known PRTG Windows Update monitoring activities to reduce future investigation effort while maintaining visibility into unauthorized encoded PowerShell executions.

## MITRE ATT&CK Mapping

| Technique | Description |
|------------|-------------|
| T1059.001 | PowerShell |

> Note: Although encoded PowerShell commands are commonly used by attackers, legitimate administration and monitoring tools may also use this technique. Context and payload analysis are required before determining malicious intent.