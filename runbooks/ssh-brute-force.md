# Runbook — SSH brute force

**Severity:** Medium (escalate if a login succeeds)
**Data source:** Wazuh agent → authentication logs
**Owner:** Abhishek Gupta (lab)

## 1. Detect

Alert: repeated SSH authentication failures against the Ubuntu target.

Check:
- Wazuh dashboard → Security events → filter `rule.groups: authentication_failed`
- Count of failures per source IP in the last 15 minutes
- Whether any failure was followed by a successful login

## 2. Triage

| Question | Why it matters |
|---|---|
| Single source or distributed? | Distributed suggests botnet / password spraying |
| Which accounts were targeted? | `root` and common names are low-sophistication |
| Any success after failures? | Possible compromise — escalate immediately |
| Is the source internal or external? | Internal changes the containment path |

## 3. Contain

- Block the source IP at the host firewall (lab only).
- Disable SSH password authentication; enforce key-based auth.
- If a login succeeded: isolate the host, rotate credentials, preserve logs.

## 4. Document

- Timeline (first failure → detection → containment).
- Rule ID and threshold used.
- What worked, what was noisy, what I would change.
- Follow-up: alert tuning, fail2ban, MFA, network segmentation.

## 5. Improve

- [ ] Reduce false positives without hiding real attempts.
- [ ] Add a dashboard panel for failed-login trends.
- [ ] Write the lesson learned in the repo README.
