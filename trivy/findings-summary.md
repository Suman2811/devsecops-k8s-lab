# Trivy Scan Results — devsecops-app

## Summary
| Severity | Count |
|----------|-------|
| CRITICAL | 0 |
| HIGH | 4 |
| MEDIUM | 0 |
| LOW | 0 |

## HIGH Findings
- **CVE-2025-69720** — ncurses buffer overflow (libncursesw6, libtinfo6, ncurses-base, ncurses-bin)
- Status: No fix available upstream
- Risk accepted — base image vulnerability, monitoring for patch

## Remediation Applied
- Upgraded pip to fix CVE-2025-8869, CVE-2026-3219, CVE-2026-6357
- Reduced total vulnerabilities from 97 to 4
