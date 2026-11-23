# Auth log field map (Linux)

Typical `auth.log` / `secure` lines used by N1 enrichment.

| Pattern | Fields |
|---------|--------|
| `Failed password for root from 203.0.113.10 port 52211 ssh2` | `result=fail`, `user`, `src_ip`, `src_port`, `proto=ssh` |
| `Failed password for invalid user admin from 198.51.100.8` | `result=fail`, `user=invalid`, `src_ip` |
| `Accepted publickey for ubuntu from 203.0.113.10` | `result=success`, `method=publickey`, `user`, `src_ip` |
| `Accepted password for alice from 10.0.0.12` | `result=success`, `method=password`, `user`, `src_ip` |

Parser implementation: `soc-log-analysis/scripts/parse_auth_log.py`.
