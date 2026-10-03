# Brute Force Attack Detection — Investigation Report

## 1. Executive Summary

 This SOC investigation examined repeated Windows authentication failures in a controlled personal laboratory environment.

Windows Security Event ID 4625 was investigated as the failed authentication event.

Wazuh was used to collect and investigate the authentication activity.

The investigation identified repeated failed authentication events associated with the `SOC-Test` account and Wazuh Rule ID `60122`.

A successful remote logon detection associated with `SOC-TEST` was also observed and correlated as additional context.

The available evidence does not by itself establish account compromise.

---

## 2. Scenario

The objective was to investigate repeated authentication failures and determine whether the observed pattern warranted investigation as possible brute-force activity.

The investigation included:

- Windows Event Viewer
- Windows Security Event Logs
- Wazuh Threat Hunting
- Event ID 4625
- Event ID 4624
- Source IP analysis
- Event correlation
- Timeline creation

---

## 3. Environment

### Windows Endpoint

- Operating System: Windows
- Hostname: VEDHAN
- Target Account: SOC-Test
- Agent Name: vedhan
- Agent ID: 002

### Wazuh

- Wazuh Manager
- Wazuh Threat Hunting
- Windows event collection

### Virtualization

- VirtualBox
- Private controlled laboratory environment

---

## 4. Failed Authentication Event

The representative Windows event investigated was:

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

The event was reviewed in Windows Event Viewer.

---

## 5. Wazuh Detection

Wazuh Threat Hunting was used to investigate the authentication activity.

Observed failed-login detection:

- Rule ID: 60122
- Rule Level: 5
- Rule Description: Logon Failure - Unknown user or bad password
- Agent: vedhan

One Wazuh result set showed 5 matching failed-login events.

Observed timestamps:

- October 3, 2026 @ 22:07:43.057
- October 3, 2026 @ 22:11:32.492
- October 3, 2026 @ 22:12:05.391
- October 3, 2026 @ 22:15:33.837
- October 3, 2026 @ 22:16:10.708

---

## 6. Source IP Analysis

The Wazuh investigation included the source IP:

`192.168.56.1`

The source was investigated in the context of the controlled private virtual environment.

The target account was:

`SOC-Test`

---

## 7. Successful Authentication Correlation

A separate Wazuh search for Event ID 4624 was performed.

The investigation identified a successful remote logon detection associated with:

- User: SOC-TEST
- Wazuh Rule ID: 92657
- Rule Level: 6
- Authentication: NTLM
- Possible RDP connection indicated by the Wazuh rule description

A later Wazuh investigation view contained 8 matching events, including repeated failed authentication events and one successful remote logon detection.

The successful authentication was treated as correlation evidence.

It was not treated as proof of compromise.

---

## 8. Investigation Timeline

| Time | Activity | Evidence |
|---|---|---|
| 22:07:43.057 | Failed authentication | Wazuh Rule 60122 |
| 22:11:32.492 | Failed authentication | Wazuh Rule 60122 |
| 22:12:05.391 | Failed authentication | Wazuh Rule 60122 |
| 22:15:33.837 | Failed authentication | Wazuh Rule 60122 |
| 22:16:10.708 | Failed authentication | Wazuh Rule 60122 |
| 22:41:08.121 | Failed authentication | Wazuh investigation |
| 22:41:59.852 | Failed authentication | Wazuh investigation |
| 22:44:10.873 | Successful remote logon detection | Wazuh Rule 92657 |

---

## 9. Event Analysis

The Windows Event ID 4625 evidence showed:

- Target account: SOC-Test
- Logon Type: 3
- Authentication Package: NTLM
- Workstation: VEDHAN
- Failure status: 0xc000006d
- Failure substatus: 0xc000006a

The Wazuh investigation provided additional SIEM-level visibility into the authentication pattern.

---

## 10. Correlation

The investigation correlated:

- Target account
- Agent
- Source IP
- Authentication events
- Event IDs
- Wazuh detection rules
- Event timestamps

The failed authentication events were reviewed as a related sequence rather than as isolated events.

The successful remote logon detection was investigated as additional context following the authentication failures.

---

## 11. Findings

### Finding 1 — Repeated Authentication Failures

Multiple failed authentication events were observed.

Wazuh identified these events using Rule ID 60122.

### Finding 2 — Target Account

The investigated account was:

`SOC-Test`

### Finding 3 — Source Information

The investigated source IP was:

`192.168.56.1`

### Finding 4 — Successful Authentication

A successful remote logon detection associated with `SOC-TEST` was observed in Wazuh.

The successful event was correlated with the investigation but was not interpreted as proof of compromise.

---

## 12. Assessment

The observed activity represents a repeated authentication-failure pattern that warranted investigation as possible brute-force activity.

The available evidence does not independently establish that the account was compromised.

The investigation therefore focuses on the observable authentication behavior and its correlation in Wazuh.

---

## 13. Response Recommendations

Recommended defensive actions for a production SOC include:

1. Validate whether the source IP is authorized.
2. Confirm whether the affected account belongs to a legitimate user or service.
3. Review authentication activity surrounding the detection.
4. Check for successful authentication after repeated failures.
5. Review related endpoint events.
6. Follow the organization's approved containment process.
7. Restrict or block the source when authorized.
8. Reset credentials when compromise is suspected and policy requires it.
9. Enable or verify MFA where supported.
10. Tune detection thresholds to reduce false positives.
11. Preserve relevant evidence.
12. Document the investigation and response.

No production containment action was performed during this personal laboratory investigation.

---

## 14. Limitations

This investigation was performed in a controlled personal laboratory environment.

The evidence represents the events observed during the investigation period.

Repeated authentication failures can have legitimate causes, including incorrect passwords, misconfigured applications, automated services, or expired credentials.

The investigation therefore does not claim compromise solely from the presence of failed authentication events.

---

## 15. Conclusion

The investigation demonstrated a practical SOC L1 workflow for Windows authentication monitoring.

The workflow included:

1. Collecting Windows authentication events.
2. Identifying Event ID 4625.
3. Investigating repeated failed authentication.
4. Identifying the affected account.
5. Investigating source information.
6. Searching the activity in Wazuh.
7. Correlating authentication events.
8. Checking for successful authentication.
9. Building an investigation timeline.
10. Documenting findings and response recommendations.

The project demonstrates hands-on experience with Windows Security Event Logs, Wazuh SIEM, authentication investigation, event correlation, and SOC documentation.

---

## 16. Evidence Screenshots

The project evidence includes:

- `01_failed_login_event_details.png`
- `02_brute_force_pattern.png`
- `03_successful_login.png`
- `04_final_investigation.png`
