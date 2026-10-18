# Wazuh SOC Lab — Detection Engineering & Incident Investigation

## Project Overview

This project documents the development of a home Security Operations Center (SOC) laboratory using Wazuh SIEM, Windows 11, Ubuntu Server, Sysmon, and VMware Workstation Pro.

The lab was designed to gain hands-on experience with security monitoring, endpoint telemetry, custom detection engineering, and incident investigation.

Three custom Wazuh detection rules were developed and tested against simulated Windows security activity.

## Lab Architecture

**Virtualization:** VMware Workstation Pro

**SIEM Server:** Ubuntu Server 24.04 LTS running Wazuh Manager

**Monitored Endpoint:** Windows 11 with Wazuh Agent

**Endpoint Telemetry:** Windows Security Event Logs, Sysmon, and PowerShell Script Block Logging

**Detection Engine:** Wazuh custom XML rules

### Architecture Diagram

Windows 11 Endpoint (Wazuh Agent + Sysmon)

↓

Windows Security, Sysmon, and PowerShell Event Logs

↓

Wazuh Manager (Ubuntu Server)

↓

Custom Detection Rules and Security Alerts

## Custom Detection Rules

### Detection 1 — PowerShell Account Enumeration (Sysmon)

- Rule ID: 100002
- Severity: Level 8
- MITRE ATT&CK: T1087.001 — Local Account Discovery

Detects PowerShell commands containing `Get-LocalUser` using Sysmon process creation telemetry.

### Detection 2 — PowerShell Script Block Account Enumeration

- Rule ID: 100003
- Severity: Level 8
- Windows Event ID: 4104
- MITRE ATT&CK: T1087.001 — Local Account Discovery

Detects local account enumeration through PowerShell Script Block Logging.

### Detection 3 — Windows Brute-Force Authentication Detection

- Rule ID: 100004
- Severity: Level 10
- Windows Event ID: 4625
- MITRE ATT&CK: T1110 — Brute Force

Correlates five failed Windows authentication attempts within 60 seconds against the same target username.

The detection was successfully validated using six simulated failed authentication attempts.

## Incident Investigation — SOC-001

**Incident:** Simulated Windows Brute-Force Authentication Activity

**Affected Endpoint:** SOC-WIN11

**Target Account:** WazuhTest

**Investigation Summary:**

A controlled authentication test generated six failed Windows login attempts.

Wazuh detected the repeated failures and triggered custom rule 100004.

The investigation included:

- Reviewing Wazuh alert metadata and severity
- Reconstructing the authentication event timeline
- Identifying the target account and authentication source
- Examining Windows Event IDs 4625, 4624, and 4740
- Checking Windows account lockout policy
- Documenting the findings and recommendations

**Outcome:** Detection successful. No successful authentication or account lockout events were identified in the reviewed time window.

See the incident report in the `incident-reports/` directory.

## Repository Structure

```text
Wazuh-SOC-Lab/
├── README.md
├── detections/
│   └── local_rules.xml
├── incident-reports/
├── screenshots/
└── documentation/
```

## Skills Demonstrated

- Security Information and Event Management (SIEM)
- Security alert triage and investigation
- Windows event log analysis
- Sysmon endpoint monitoring
- PowerShell security monitoring
- Custom Wazuh rule development
- Event correlation and detection engineering
- MITRE ATT&CK mapping
- Incident documentation
- VMware virtualization and Linux administration

## Future Improvements

- Expand detection coverage for additional Windows attack techniques
- Investigate suspicious PowerShell execution
- Create additional SOC incident reports
- Develop detection testing and validation procedures
- Improve security dashboards and alert visualization

## Disclaimer

This project was conducted in a controlled home laboratory for educational and defensive security purposes. All suspicious activity was intentionally simulated.
