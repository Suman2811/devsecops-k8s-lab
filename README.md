# DevSecOps Kubernetes Security Lab
# DevSecOps Kubernetes Security Lab

A complete DevSecOps pipeline integrating security at every
stage — from code commit to runtime monitoring.

## Architecture

Code Push → GitHub Actions → Kubernetes → Runtime Monitoring
│
├── Gitleaks    (secrets scan)
├── Semgrep     (SAST — code vulnerabilities)
├── Docker build
└── Trivy       (container image CVE scan)
│
kubectl deploy
│
┌──────────────────────┐
│  Kubernetes Cluster   │
│  ┌────────────────┐  │
│  │  devsecops-app │  │
│  └────────────────┘  │
│  kube-bench (CIS)     │
│  Falco (runtime)      │
└──────────────────────┘

## Pipeline Stages

| Stage | Tool | Result |
|-------|------|--------|
| Secrets scanning | Gitleaks | Blocks committed API keys/passwords |
| SAST | Semgrep | Caught Flask host exposure — fixed |
| Container build | Docker | python:3.12-slim base image |
| Image scanning | Trivy | 97 CVEs reduced to 4 |
| CIS benchmark | kube-bench | 57 pass / 14 fail documented |
| Runtime detection | Falco | MITRE T1059 alert fired |

## Key Security Findings

### Semgrep — SAST
- Finding: Flask app running with host=0.0.0.0 exposes server publicly
- Fix: Restricted to host=127.0.0.1
- Commit: Fix: restrict Flask host to localhost (Semgrep finding)

### Trivy — Container Scanning
- Before: 97 vulnerabilities (HIGH: 4, MEDIUM: 29, LOW: 63)
- After: 4 vulnerabilities (all HIGH, no fix available upstream)
- Remediated: pip CVE-2025-8869, CVE-2026-3219, CVE-2026-6357

### kube-bench — CIS Benchmark
- Total: 131 checks — 57 PASS / 14 FAIL / 60 WARN
- Critical FAILs:
  - Audit logging not enabled (1.2.16-19)
  - etcd data directory permissions (1.1.11-12)
  - Profiling enabled on API server (1.2.15)
  - Kubelet service file permissions (4.1.1)

### Falco — Runtime Detection
- Rule fired: Terminal shell in container
- MITRE ATT&CK: T1059 — Command and Scripting Interpreter
- Triggered by: exec into pod and attempted /etc/shadow read
- Pod: devsecops-app, User UID: 65534

## Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| Docker | 29.4.3 | Container runtime |
| Minikube | v1.38.1 | Local Kubernetes cluster |
| kubectl | v1.34.1 | Kubernetes CLI |
| Helm | latest | Kubernetes package manager |
| Trivy | v0.70 | Container vulnerability scanner |
| Semgrep | v1.36.0 | Static application security testing |
| kube-bench | latest | CIS Kubernetes benchmark |
| Falco | v0.43.1 | Runtime threat detection |
| GitHub Actions | — | CI/CD pipeline |

## Project Structure
devsecops-k8s-lab/
├── .github/workflows/devsecops.yml   CI/CD pipeline
├── app/app.py                         Flask application
├── app/Dockerfile                     Hardened container image
├── k8s/deployment.yaml                Kubernetes manifest
├── trivy/scan-report.json             Full Trivy output
├── trivy/findings-summary.md          CVE analysis
├── kube-bench/audit-results.txt       Full CIS benchmark output
├── kube-bench/findings-summary.md     Top findings and remediation
├── falco/falco-alerts.txt             Captured runtime alerts
├── falco/findings-summary.md          Alert analysis
└── report.md                          Full security assessment
## How to Run

```bash
minikube start --driver=docker
kubectl apply -f k8s/deployment.yaml
kubectl get pods
kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job.yaml
kubectl logs job/kube-bench
trivy image devsecops-app:latest
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm install falco falcosecurity/falco --namespace falco --create-namespace --set driver.kind=modern_ebpf --set tty=true --set falco.log_stderr=true
kubectl exec -it <pod-name> -- /bin/sh -c "cat /etc/shadow"
kubectl logs -n falco -l app.kubernetes.io/name=falco -c falco --since=5m
```

## CV Line

Built a DevSecOps pipeline with SAST (Semgrep), secrets scanning (Gitleaks), and container scanning (Trivy) in GitHub Actions, deploying to a hardened Kubernetes cluster with CIS benchmark compliance (kube-bench) and Falco runtime detection — confirmed MITRE T1059 alert.

## Certifications This Project Maps To

- CKS (Certified Kubernetes Security Specialist) — 60% syllabus coverage
- AWS Security Specialty — container security concepts
- CompTIA Security+ — vulnerability management and threat detection
EOF
