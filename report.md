# DevSecOps Kubernetes Security Lab — Report

## Project Summary
Built a complete DevSecOps pipeline integrating security
at every stage from code commit to runtime.

## Pipeline (GitHub Actions)
- SAST: Semgrep caught Flask host exposure vulnerability
- Secrets: Gitleaks scanning on every commit
- Image scan: Trivy reduced vulnerabilities from 97 to 4

## Kubernetes Security
- CIS Benchmark: 57 pass, 14 fail — top findings documented
- RBAC: Reviewed cluster-admin bindings
- Runtime: Falco deployed and monitoring all pod syscalls

## Key Findings & Remediations
| Finding | Severity | Status |
|---------|----------|--------|
| Flask host 0.0.0.0 | HIGH | Fixed |
| pip CVE-2025-8869 | MEDIUM | Fixed |
| Audit logging disabled | HIGH | Documented |
| etcd dir permissions | HIGH | Documented |
| ncurses CVE-2025-69720 | HIGH | No fix available |

## Tools Used
Wireshark, Zeek, Suricata, Trivy, kube-bench,
Falco, Helm, Minikube, kubectl, GitHub Actions, Semgrep

## CV Line
"Built a DevSecOps pipeline with SAST, secrets scanning,
and container scanning in GitHub Actions, deploying to a
hardened Kubernetes cluster with Falco runtime detection
and CIS benchmark compliance."
