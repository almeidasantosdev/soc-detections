# SOC Detections — Rules, Queries & Triage Map

Detection content for **SOC N1** practice: Sigma rules, Microsoft Sentinel **KQL**, Splunk **SPL**, simple log parsers, and an **alert → severity → action** matrix used during triage and escalation.

Companion repos:

- [soc-homelab](https://github.com/almeidasantosdev/soc-homelab) — monitoring lab  
- [soc-n1-playbooks](https://github.com/almeidasantosdev/soc-n1-playbooks) — incident runbooks  
- [soc-log-analysis](https://github.com/almeidasantosdev/soc-log-analysis) — parsers & FP reports  

---

## Alert → severity → action map

| Alert family | Default severity | N1 action | Escalate to N2 when |
|--------------|------------------|-------------|---------------------|
| SSH / RDP brute force (failures only) | Medium | Confirm source, count, targets; check VPN/scanner allowlist; ticket | ≥1 successful auth from same source **or** ≥3 hosts targeted |
| Auth success after failures | High | Preserve evidence, disable/reset if playbook allows, page N2 | Always (credential compromise suspected) |
| Firewall port scan (deny only) | Low | Document ports/rate; enrich IP | Internal source **or** allow follows deny pattern |
| Firewall allow to admin ports from internet | High | Block request / verify rule; open incident | Confirmed exposure or active session |
| Endpoint malware / EDR block | High | Isolate if authorized; collect hash/path; follow malware playbook | Always for ransomware/C2 confirmed |
| Phishing report (user) | Medium | Quarantine mail, check clicks/creds | Credentials entered **or** malware attachment executed |
| FIM on critical config | High | Correlate change ticket / admin session | No ticket + suspicious process |

Full matrix: [`triage/alert-action-map.md`](triage/alert-action-map.md).

---

## Contents

```
soc-detections/
├── sigma/                 # Detection-as-code (YAML)
├── kql/                   # Microsoft Sentinel / Azure Log Analytics
├── spl/                   # Splunk Search Processing Language
├── parsers/               # Lightweight field extractors
└── triage/
    └── alert-action-map.md
```

---

## Sigma rules

| File | Intent |
|------|--------|
| `sigma/linux_ssh_bruteforce.yml` | Multiple SSH failures from one source |
| `sigma/linux_ssh_success_after_failures.yml` | Success correlated after failures |
| `sigma/windows_rdp_bruteforce.yml` | Failed logons 4625 (LogonType 3/10) |
| `sigma/network_port_scan_denies.yml` | Burst of firewall denies |

Convert with [sigma-cli](https://github.com/SigmaHQ/sigma-cli) to your SIEM backend when needed.

---

## Example KQL (Sentinel)

See `kql/ssh_bruteforce.kql` and `kql/firewall_port_scan.kql`.

## Example SPL (Splunk)

See `spl/ssh_bruteforce.spl` and `spl/firewall_port_scan.spl`.

---

## How an N1 uses this repo

1. Alert fires in SIEM/homelab  
2. Match family in the action map  
3. Run supporting query (KQL/SPL) to enrich  
4. Open the matching playbook in `soc-n1-playbooks`  
5. Close as FP or escalate with evidence package  

---

## Disclaimer

Educational content. Rules are intentionally simple for learning; tune thresholds before any production use.
