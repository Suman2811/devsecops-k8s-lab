# kube-bench CIS Benchmark Audit

## Summary
| Result | Count |
|--------|-------|
| PASS   | 57    |
| FAIL   | 14    |
| WARN   | 60    |

## Critical FAIL Findings

### 1.2.16-19 — Audit Logging Not Enabled (HIGH)
- No audit log path, maxage, maxbackup, or maxsize configured
- Risk: Security events are not recorded — attackers can act without trace
- Fix: Add --audit-log-path=/var/log/apiserver/audit.log to kube-apiserver

### 1.2.15 — Profiling Enabled on API Server (MEDIUM)
- Risk: Exposes sensitive system and program details
- Fix: Set --profiling=false on kube-apiserver

### 1.1.11-12 — etcd Data Directory Permissions (HIGH)
- Risk: etcd stores all cluster secrets — wrong permissions risk exposure
- Fix: chmod 700 /var/lib/etcd && chown etcd:etcd /var/lib/etcd

### 4.1.1 — Kubelet Service File Permissions (MEDIUM)
- Risk: Kubelet config could be tampered with
- Fix: chmod 600 /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

## Note
Some findings are expected in a Minikube dev environment
and would be remediated differently in production.
