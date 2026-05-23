# Falco Runtime Security

## Deployment
- Falco v0.43.1 deployed via Helm to dedicated falco namespace
- Using modern_ebpf driver on Minikube (ARM64/M1)
- Monitoring syscall events across all pods

## Status
- Falco running: 2/2 containers Ready
- Rules loaded: falco_rules.yaml (default ruleset)
- Event source: syscall

## Test Actions Performed
- Exec'd into devsecops-app pod
- Read /etc/passwd (sensitive file access)
- Ran find / filesystem enumeration (recon behaviour)

## Note
Alert stdout output requires additional config on Minikube ARM64.
In production, alerts forward to SIEM via falcosidekick.
