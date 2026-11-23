# Firewall log field map

Normalize vendor logs to a common schema before correlation.

| Normalized field | Meaning | Example values |
|------------------|---------|----------------|
| `src_ip` | Source | `198.51.100.44` |
| `dst_ip` | Destination | `10.10.0.5` |
| `dst_port` | Service port | `22`, `3389` |
| `action` | Policy result | `allow`, `deny`, `drop` |
| `proto` | Protocol | `tcp`, `udp` |
| `rule_id` | Policy identifier | `ACL-443-IN` |
| `device` | Firewall hostname | `fw-edge-01` |

Minimum for port-scan detection: `src_ip`, `dst_port`, `action`, timestamp.
