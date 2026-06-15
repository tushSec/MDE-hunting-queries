# Rare Process Hunting

## Overview

Rare process hunting helps identify executables that run on only a small number of devices within the environment. Malware, unauthorized tools, and attacker-deployed binaries often have limited prevalence and may appear as processes observed on only a few endpoints.

This hunting technique focuses on identifying low-frequency process executions that warrant further investigation.

## Detection Query

```kusto
DeviceProcessEvents
| summarize DeviceCount = dcount(DeviceName) by FileName
| where DeviceCount < 5
| order by DeviceCount asc
```

## Use Case

Malware often appears as processes that execute on very few devices. By identifying low-prevalence processes, defenders can uncover:

* Malware and post-exploitation tools
* Unauthorized software installations
* Custom attacker tooling
* Suspicious scripts or binaries
* Potential insider activity
* Newly introduced applications that have not been approved

## Investigation Steps

1. Review the process name and prevalence across the environment.
2. Identify the devices where the process executed.
3. Examine the process command line and parent process.
4. Verify the file path and execution context.
5. Check file hashes and digital signatures.
6. Review associated network connections and child processes.
7. Determine whether the process is part of approved business software.
8. Submit unknown binaries for malware analysis if required.

## Findings

* Processes running on fewer than five devices were identified.
* Low-prevalence processes are not inherently malicious but represent candidates for further investigation.
* Additional context such as file reputation, signer information, execution path, and user activity is required to determine risk.

## Classification

| Category         | Value                                |
| ---------------- | ------------------------------------ |
| Alert Type       | Rare Process Execution               |
| Detection Method | Process Prevalence Analysis          |
| Severity         | Medium (Context Dependent)           |
| Impact           | Requires Investigation               |
| Verdict          | Investigate Low-Prevalence Processes |

## Recommendation

* Establish a baseline of approved applications within the environment.
* Investigate unsigned or unknown executables with low prevalence.
* Correlate findings with threat intelligence, file reputation, and device activity.
* Monitor for recurring executions of previously unseen processes.
* Consider allow-listing known business applications to reduce false positives.

## MITRE ATT&CK Mapping

| Technique | Description                       |
| --------- | --------------------------------- |
| T1036     | Masquerading                      |
| T1059     | Command and Scripting Interpreter |
| T1105     | Ingress Tool Transfer             |
| T1204     | User Execution                    |

> Note: Rare processes are not automatically malicious. Administrative tools, newly deployed software, and internally developed applications may also exhibit low prevalence. Validation and contextual analysis are essential before determining malicious intent.
