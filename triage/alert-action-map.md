# Alert → severity → action (N1)

Operational matrix for first-line SOC.

## Severity legend

| Level | Meaning | Typical SLA acknowledge |
|-------|---------|-------------------------|
| Low | Recon / noisy deny | Queue SLA |
| Medium | Active attack attempt, no confirmed impact | Priority queue |
| High | Likely compromise or critical exposure | Immediate |
| Critical | Confirmed impact / ransomware / mass lockouts | Page N2/N3 |

## Matrix

| ID | Alert | Sev | N1 steps | Escalate if |
|----|-------|-----|----------|-------------|
| A1 | SSH/RDP brute (failures) | M | Validate count & targets; enrich IP; ticket | Success from same IP; multi-host |
| A2 | Success after failures | H | Snapshot sessions; contain per playbook; page N2 | Always |
| A3 | FW port scan (deny) | L | Document; check approved scanners | Internal src; allow after deny |
| A4 | FW allow admin from Internet | H | Verify rule; request block; incident | Session established |
| A5 | EDR malware blocked | H | Hash/path; isolation if allowed | Family = ransomware/C2 |
| A6 | EDR malware not blocked | C | Isolate; page N2; preserve volatile data | Always |
| A7 | User-reported phishing | M | Quarantine; check click/credential | Creds entered / payload run |
| A8 | FIM critical path | H | Match change ticket | No ticket + odd parent process |

## Evidence package for N2

- Alert ID + raw event  
- Timeline (±30 min)  
- Asset owner / criticality  
- Related detections  
- Actions already taken by N1  
- Recommended next step  
