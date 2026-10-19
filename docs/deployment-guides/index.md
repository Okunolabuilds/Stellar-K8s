# Deployment Guides

Guides for deploying Stellar nodes and their supporting services on Kubernetes with the Stellar-K8s operator.

## Choose a guide

| Guide | Use it when |
|-------|-------------|
| [Validator Node](./validator.md) | Running a Stellar Core validator (quorum sets, history archives, production options). |
| [Horizon API](./horizon.md) | Serving the Horizon REST API in front of a Stellar Core node. |
| [Soroban RPC](./soroban-rpc.md) | Running a Soroban RPC endpoint for smart-contract clients. |

## Before you deploy

1. Install the operator and CRDs: [Installation](../getting-started/installation.md).
2. Run the end-to-end local flow first if you have no cluster yet: [Quick Start](../getting-started/quick-start.md).
3. Review production hardening: [Production Security Hardening](../production-security-hardening.md) and [Network Isolation](../network-isolation.md).
4. Plan storage growth: [Proactive Disk Scaling](../proactive-disk-scaling.md) and [Archive Pruning](../archive-pruning.md).

## Patterns and walkthroughs

- [Deployment patterns](../deployment-patterns/index.md): validator HA topology, Horizon RPC farms, multi-region DR, GitOps scaling and upgrades.
- [Deploy a testnet validator](../tutorials/deploy-testnet-validator.md): step-by-step tutorial.
- [Configure HA setup](../tutorials/configure-ha-setup.md): high-availability configuration.
- [Backup and restore](../tutorials/backup-restore.md): data protection for node state.

## After deployment

- Monitor node health with the [Observability overview](../observability/index.md).
- Upgrade the operator and nodes with the [Upgrade guide](../operations/upgrade-guide.md).
- Troubleshoot with [Common Issues](../troubleshooting/common-issues.md).
