# Brute Force Attack Detection

SOC / Blue Team portfolio project focused on detecting and investigating repeated Windows authentication failures using Windows Security Event Logs and Wazuh.

> **Project Type:** Independent cybersecurity portfolio project — not part of the Enterprise SOC Home Lab.

---

## 🎯 Project Objective

The objective of this project is to demonstrate a practical SOC Analyst L1 investigation of repeated authentication failures in a controlled Windows environment.

The investigation covers:

- Windows Security Event Logs
- Failed authentication detection
- Successful authentication correlation
- Source IP investigation
- Wazuh SIEM investigation
- Event correlation
- Incident timeline development
- Evidence preservation
- SOC investigation documentation

All testing was performed in a controlled personal laboratory environment.

---

## 🖥️ Environment

### Windows Endpoint

- Operating System: Windows
- Hostname observed in event data: VEDHAN
- Target Account: SOC-Test
- Agent Name: vedhan
- Agent ID: 002

### SIEM

- Wazuh
- Wazuh Threat Hunting
- Windows Security Event Logs

### Virtualization

- VirtualBox
- Controlled private laboratory environment

---

## 🛠️ Technologies Used

- Windows Event Viewer
- Windows Security Event Logs
- Wazuh SIEM
- Wazuh Threat Hunting
- Windows authentication events
- Event ID 4625
- Event ID 4624
- VirtualBox
- GitHub
- Markdown

---

## 🔎 Investigation Scenario

Repeated authentication failures were investigated against the controlled Windows laboratory environment.

The investigation focused on determining:

1. Whether repeated failed authentication events were present.
2. Which account was targeted.
3. What source information was available.
4. Which Windows event IDs were involved.
5. Whether a successful authentication occurred afterward.
6. How the activity appeared in Wazuh.
7. Whether the observed pattern warranted investigation as possible brute-force activity.

Repeated authentication failures alone do not automatically prove account compromise.

Legitimate causes such as incorrect passwords, misconfigured applications, or automated services can also generate failed authentication events.

---

## 🚨 Detection

Windows Event ID **4625** was investigated as the failed-logon event.

The Windows Event Viewer evidence identified:

- Event ID: 4625
- Target User: SOC-Test
- Target Domain: -
- Logon Type: 3
- Logon Process: NtLmSsp
- Authentication Package: NTLM
- Workstation: VEDHAN
- Status: 0xc000006d
- SubStatus: 0xc000006a
- Failure Reason: %%2313

These fields were reviewed from the Windows Event Viewer event details.

---

## 📊 Wazuh Investigation

Wazuh Threat Hunting was used to investigate the Windows authentication activity.

A search for Event ID **4625** returned repeated failed-login events.

One investigated Wazuh result set showed:

- Agent: vedhan
- Agent ID: 002
- Rule ID: 60122
- Rule Level: 5
- Rule Description: Logon Failure - Unknown user or bad password
- Matching events: 5

Observed failed-login timestamps included:

- October 3, 2026 @ 22:07:43.057
- October 3, 2026 @ 22:11:32.492
- October 3, 2026 @ 22:12:05.391
- October 3, 2026 @ 22:15:33.837
- October 3, 2026 @ 22:16:10.708

The events were reviewed as a sequence rather than as isolated alerts.

---

## 🌐 Source IP Investigation

The Wazuh investigation was filtered using the source IP information.

The investigated source IP was:

**192.168.56.1**

Additional observed information:

- Target Account: SOC-Test
- Agent: vedhan
- Agent ID: 002

The source information was reviewed in the context of the controlled private virtual laboratory environment.

---

## 🔐 Successful Authentication Correlation

A separate Wazuh search for Event ID **4624** was performed to investigate successful authentication activity.

The Wazuh results included a successful remote logon detection associated with:

- User: SOC-TEST
- Wazuh Rule ID: 92657
- Rule Level: 6
- Authentication: NTLM
- Possible RDP connection context indicated by the Wazuh rule description

The successful authentication was investigated as additional context following the failed authentication activity.

A successful login after failed authentication attempts does not by itself prove account compromise.

---

## 🕒 Investigation Timeline

The following timestamps were observed in the captured Wazuh evidence:

| Time | Activity | Evidence |
|---|---|---|
| 22:07:43.057 | Failed authentication | Wazuh Rule 60122 |
| 22:11:32.492 | Failed authentication | Wazuh Rule 60122 |
| 22:12:05.391 | Failed authentication | Wazuh Rule 60122 |
| 22:15:33.837 | Failed authentication | Wazuh Rule 60122 |
| 22:16:10.708 | Failed authentication | Wazuh Rule 60122 |
| 22:41:08.121 | Failed authentication | Wazuh investigation view |
| 22:41:59.852 | Failed authentication | Wazuh investigation view |
| 22:44:10.873 | Successful remote logon detection | Wazuh Rule 92657 |

---

## 🔬 Event Analysis

The Windows Event ID 4625 evidence was reviewed to understand the authentication failure.

Important observed fields included:

| Field | Value |
|---|---|
| Event ID | 4625 |
| Target User | SOC-Test |
| Target Domain | - |
| Status | 0xc000006d |
| Failure SubStatus | 0xc000006a |
| Logon Type | 3 |
| Logon Process | NtLmSsp |
| Authentication Package | NTLM |
| Workstation | VEDHAN |

The event represents a failed Windows authentication attempt.

---

## 🧠 Detection Logic

The investigation follows this detection concept:

```text
Multiple authentication failures
        ↓
Same or related target account
        ↓
Relevant time period
        ↓
Investigate source information
        ↓
Check for successful authentication
        ↓
Correlate events
        ↓
Determine whether the pattern is suspicious
