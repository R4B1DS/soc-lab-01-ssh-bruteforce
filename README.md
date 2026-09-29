# SOC Lab 01: SSH Brute Force Investigation

A hands-on Blue Team lab simulating the work of a Tier 1 SOC analyst: detecting, investigating, and responding to an SSH brute force attack.

![Focus](https://img.shields.io/badge/Focus-SOC_%7C_Blue_Team-blue?style=flat-square)
![Level](https://img.shields.io/badge/Level-Junior-green?style=flat-square)
![MITRE](https://img.shields.io/badge/MITRE_ATT%26CK-T1110-red?style=flat-square)

## Scenario

The SOC received an alert for a high number of failed SSH authentication attempts against a corporate Linux server. As the analyst on duty, the goal is to determine whether this is a real attack, whether it succeeded, and what actions must be taken.

## Objectives

- Identify the source of the attack and the targeted accounts
- Determine whether any login attempt succeeded (compromise)
- Build an event timeline
- Map the activity to MITRE ATT&CK
- Recommend containment and hardening actions
- Document the incident in a technical report and SOC ticket

## Skills Demonstrated

- Authentication log analysis (`auth.log`)
- Indicator of Compromise (IOC) extraction
- Attack timeline reconstruction
- MITRE ATT&CK mapping
- Incident response (containment, eradication, recovery)
- Technical reporting and SOC ticketing

## Tools

- Linux command line (`grep`, `awk`, `sort`, `uniq`)
- Log analysis of `/var/log/auth.log`
- MITRE ATT&CK framework

## Methodology

1. **Triage:** confirm the alert and define the time window
2. **Log analysis:** identify failed logins, source IPs, and targeted usernames
3. **Success check:** search for any accepted login from the suspicious source
4. **Timeline:** order events from first attempt to last
5. **Classification:** map to MITRE ATT&CK and assess severity
6. **Response:** define containment and prevention actions
7. **Documentation:** report and ticket

### Example: counting failed attempts per source IP

```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head
```

### Example: checking for successful logins

```bash
grep "Accepted password" /var/log/auth.log
```

## Key Findings

> Replace the `<...>` values with the results of your investigation.

| Item | Result |
|---|---|
| Attack type | SSH brute force (password guessing) |
| Source IP(s) | `<IP>` |
| Total failed attempts | `<number>` |
| Targeted accounts | `<usernames>` |
| Time window | `<start>` to `<end>` |
| Successful login? | `<Yes / No>` |
| Severity | `<Low / Medium / High>` |
| Verdict | `<True positive / False positive>` |

## Timeline

| Time | Event |
|---|---|
| `<time>` | First failed attempt from `<IP>` |
| `<time>` | Attempts intensify against `<account>` |
| `<time>` | Last recorded attempt |
| `<time>` | Alert triggered / analyst triage |

## Indicators of Compromise (IOCs)

| Type | Value |
|---|---|
| IP address | `<IP>` |
| Usernames tried | `<list>` |
| Log pattern | `Failed password for ... from <IP> port ... ssh2` |

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Brute Force | [T1110](https://attack.mitre.org/techniques/T1110/) |
| Credential Access | Brute Force: Password Guessing | [T1110.001](https://attack.mitre.org/techniques/T1110/001/) |
| Initial Access | Remote Services: SSH | [T1021.004](https://attack.mitre.org/techniques/T1021/004/) |

## Incident Response

**Containment**
- Block the source IP at the firewall
- Lock or reset targeted accounts if any were compromised

**Eradication and Recovery**
- Confirm no unauthorized sessions or persistence exist
- Review logs for the period before and after the attack

**Prevention**
- Disable SSH password authentication and use key-based authentication
- Deploy `fail2ban` or equivalent rate limiting
- Restrict SSH access by IP or through a VPN
- Disable direct root login
- Create a SIEM alert for repeated failed logins from a single source

## Project Structure

```text
soc-lab-01-ssh-bruteforce/
├── README.md
├── docs/
│   ├── investigation.md
│   ├── incident-report.md
│   └── soc-ticket.md
└── evidence/
    └── (log samples and screenshots)
```

## Interview Questions This Lab Prepares For

- How would you tell a brute force attack from a user mistyping a password?
- How do you confirm whether an attack succeeded?
- Which MITRE ATT&CK techniques apply here?
- What detection rule would you create in a SIEM?

## Author

**Nicolas Borges Ocampos**
Cybersecurity student | Aspiring SOC / Blue Team analyst
[LinkedIn](https://www.linkedin.com/in/nicolas-borges-ocampos/)
