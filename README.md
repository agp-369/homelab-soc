# homelab-soc

**A small, honest home SOC: Wazuh SIEM + one agent + one alert + one mini-incident runbook.**

> **Status: building.** This is a learning lab, not a production SOC. No real IPs, no employer data, no live targets.

## Goal

Prove I can do the L1 loop end-to-end:

**detect → triage → contain → document → improve the rule.**

## Architecture

```
+-------------------+        +----------------------+        +-------------------+
|  Ubuntu target    |  logs  |  Wazuh manager       |  UI    |  Analyst (me)     |
|  (agent installed)| -----> |  (rules + alerts)    | -----> |  triage + runbook |
+-------------------+        +----------------------+        +-------------------+
          ^
          | simulated attack (SSH brute force) from a local Kali VM
```

| Component | Role |
|---|---|
| Wazuh manager | Collects logs, applies rules, raises alerts |
| Wazuh agent | Ships auth/syslog from the Ubuntu target |
| Kali VM (local) | Generates the simulated brute-force traffic |
| Ubuntu target | The monitored host |

## Scenario 1 — SSH brute force

1. Enable SSH on the Ubuntu target (local network only).
2. Run a controlled brute-force simulation from the Kali VM.
3. Confirm Wazuh raises an authentication-failure alert.
4. Triage: source, target, count, time window, success/failure.
5. Contain: block the source, disable password auth, enforce keys.
6. Document: write the runbook and the lesson learned.

Runbook: [`runbooks/ssh-brute-force.md`](runbooks/ssh-brute-force.md)

## What I will publish

- Sanitised screenshots of the alert and the dashboard.
- The detection rule (or the built-in rule ID used).
- A short incident writeup: what happened, what I did, what I changed.
- What I would do differently at 3 a.m. with no senior on call.

## Rules

- Local VMs only. No scanning networks I do not own.
- Redact public IPs, hostnames, usernames, and tokens.
- Honest status: this is a lab. Never call it production experience.

## Related

- [`security-labs`](https://github.com/agp-369/security-labs) — web/OS lab writeups.
- [`llm-security-labs`](https://github.com/agp-369/llm-security-labs) — OWASP LLM Top 10.

> "I find what breaks, fix it, measure it, and make sure it stays fixed."
