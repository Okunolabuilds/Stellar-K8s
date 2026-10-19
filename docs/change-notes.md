# Change Notes & Release Tracking

This document outlines the change tracking conventions, release note procedures, and highlights for recent and unreleased features in the Stellar-K8s operator repository.

## Overview

Stellar-K8s maintains a clear, structured record of code and configuration changes to support transparent releases and seamless upgrades for cluster operators:

1. **[CHANGELOG.md](../CHANGELOG.md)** — The primary log of all notable features, bug fixes, and breaking changes following [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) format.
2. **[RELEASE_CHECKLIST.md](../RELEASE_CHECKLIST.md)** — The pre-release verification checklist ensuring testing, security scanning, and documentation gates pass before a release cut.
3. **[Release Process](release-process.md)** — The end-to-end automated and manual procedures for chart version bumps, git tagging, and GitHub Release notes publishing.

---

## Unreleased Highlights

The following changes are currently staged in the `Unreleased` section:

### 1. Helm Chart Flags (#1702)

The operator Helm chart has been enhanced with granular flags to control operator execution modes without manual template overrides:

- **`operator.dryRun`**: Enables dry-run simulation mode (`--dry-run` flag and `DRY_RUN: "true"` environment variable). Allows cluster operators to observe reconciliation logic without writing changes to the Kubernetes API.
- **`operator.appArmorEnabled`**: Explicit configuration toggle for enabling AppArmor profile annotations on generated pods (`STELLAR_APPARMOR_ENABLED: "true"`).
- **`operator.logFormat`**: Configures log serialization format (`json` or `pretty`) via `--log-format` and `LOG_FORMAT`.
- **Validation**: Schema-validated in `charts/stellar-operator/values.schema.json` and unit-tested in `charts/stellar-operator/tests/deployment_test.yaml`.

### 2. Sample Manifests (#1703)

Additional canonical sample manifests have been provided under `config/samples/` for day-to-day operations and testing:

- **`sample-maintenance-plan.yaml`**: Demonstrates declarative `MaintenancePlan` orchestration, including node selection, parallel drain control, PodDisruptionBudget surge recovery, and SLO safety abort thresholds.
- **`sample-dryrun-validator.yaml`**: Demonstrates a testnet `StellarNode` validator configured for safe rollout verification.
- **`sample-policy-bundle.yaml`**: Demonstrates a signed `StellarPolicyBundle` specification for admission-time policy enforcement.
- **Index**: Registered and documented in [`config/samples/README.md`](../config/samples/README.md).

### 3. Change Notes Documentation (#1704)

- Established dedicated documentation in `docs/change-notes.md` detailing release notes generation, unreleased change tracking, and cross-references.
- Registered in documentation site navigation (`mkdocs.yml`) under the Contributing section.

### 4. Bats Test Coverage (#1705)

- Added unit and smoke tests in `scripts/tests/secret-rotation-check.bats` for `scripts/secret-rotation-check.sh`.
- Covers command-line argument validation (`--dry-run`, `--namespace`, `--secret`, `--deployment`, `--window`), missing `kubectl` handling, baseline zero-replica failure paths, and mock rotation checks.
- Automatically included in `make test-bats` and the GitHub Actions `Bats Suites` CI job.

---

## Change Note Maintenance Guidelines

Contributors and maintainers should observe the following guidelines when introducing changes:

1. **Atomic Commits**: Follow [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `chore:`).
2. **Unreleased Section**: When adding user-facing features or fixing bugs, append a bullet point to the `## Unreleased` section of `CHANGELOG.md` referencing the issue or PR number.
3. **Cross-References**: Ensure referenced documentation paths, script indices, and Makefile targets are kept up to date.
4. **Validation**: Validate documentation links with `python scripts/check-links.py` and navigation completeness with `python scripts/check_mkdocs_nav.py`.

---

## Related Documentation

- [Contributing Guide](../CONTRIBUTING.md) — Contributor workflow and PR guidelines
- [Release Process Guide](release-process.md) — Chart versioning and tag release automation
- [Operator Configuration](configuration/operators.md) — Operator environment variables and flags
- [Documentation Index](README.md) — Overview of available documentation topics
