# Rare Processes

Identify processes executed on fewer than 5 devices.

```kusto
DeviceProcessEvents
| summarize DeviceCount=dcount(DeviceName) by FileName
| where DeviceCount < 5
| order by DeviceCount asc
```

## Use Case

Helps identify uncommon tools, malware, or unauthorized software.
