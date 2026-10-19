# Scripts Reference

Developer and CI helper scripts live in `scripts/`. Library helpers shared by
several scripts are in `scripts/lib/` (`common.sh`, `errors.sh`,
`health-steps.sh`, `batch.sh`). Bats tests for the shell helpers are in
`scripts/tests/`. Legacy one-off scripts are archived under
`scripts/archive/` (see `scripts/archive/README.md`).

Run the standard gates locally before pushing:

```bash
bash scripts/preflight.sh        # required binaries and versions
make health-fast                 # fmt + clippy + compile (scripts/validate.sh)
make health                      # full gate incl. docs and links
make ci-local                    # fmt-check lint audit test build link-check
```

## Environment setup

| Script | Purpose | Usage |
| --- | --- | --- |
| `setup-linux.sh` | Repeatable Ubuntu 22.04+/Debian 12+ dev bootstrap (pinned Rust 1.92, kind 0.24.0) | `bash scripts/setup-linux.sh` |
| `setup-mac.sh` | Repeatable macOS dev bootstrap, skips steps already satisfied | `bash scripts/setup-mac.sh` |
| `preflight.sh` | Verifies required binaries/versions; optional `--strict`, `--ci`, `--labels` modes | `bash scripts/preflight.sh [--strict\|--ci]` |

## Health gates and validation

| Script | Purpose | Usage / CI caller |
| --- | --- | --- |
| `repo-health.sh` | Single entry point for repo health checks (`--fast`, `--with-audit`, `--with-links`, `--with-helm`) | `bash scripts/repo-health.sh`; `make health` / `make health-fast` |
| `validate.sh` | Fast local validation (delegates to `repo-health.sh --fast`) | `bash scripts/validate.sh`; `make validate` |
| `release-gate.sh` | Release validation mirroring `RELEASE_CHECKLIST.md` | `VERSION=1.2.0 bash scripts/release-gate.sh` |
| `check-links.py` | Internal markdown file+anchor link check | `python3 scripts/check-links.py`; `make link-check`; `ci.yml` docs-link-check |
| `check-stale-samples.sh` | Flags sample manifests that drifted from sources | `ci.yml` |
| `crd_migration_lint.py` | Lints CRD migrations | `python3 scripts/crd_migration_lint.py --against origin/main`; `quickstart-validation.yml` |

## Security and supply chain

| Script | Purpose | Usage / CI caller |
| --- | --- | --- |
| `check-secrets.sh` | Secret-handling checks for CI/runtime command paths | `bash scripts/check-secrets.sh`; `ci.yml` |
| `secret-rotation-check.sh` | Verifies secret rotation without downtime (`--dry-run` for CI) | `quickstart-validation.yml` |
| `audit-features.sh` | Static audit for unused crate features and dead imports (`--report`) | `bash scripts/audit-features.sh --report`; `ci.yml` |
| `dead-code-report.sh` | Periodic dead-code report to `target/reports/dead-code-report.md` (always exits 0) | `dead-code-report.yml` |
| `generate-third-party-licenses.sh` | Regenerates `THIRD_PARTY_LICENSES.md` (requires `cargo-license`) | `make third-party-licenses`; `make check-third-party-licenses` in CI |

## Helm charts and CRDs

| Script | Purpose | Usage / CI caller |
| --- | --- | --- |
| `check-chart-diff.sh` | Detects unintended Helm chart template drift | `ci.yml` |
| `check-crd-compatibility.sh` | Validates CRD schema backward compatibility and sample manifests | `ci.yml` |
| `check-benchmark-sanity.sh` | Reproducible benchmark sanity checks for PR pipelines | `ci.yml` |
| `generate-api-docs.py` | Generates `docs/api-reference.md` from the CRD schema | `make generate-api-docs`; `make check-api-docs` in CI |

## Operations and diagnostics

| Script | Purpose | Usage |
| --- | --- | --- |
| `debug-network.sh` | In-cluster networking diagnostics (connection refused, no route, SCP) | `bash scripts/debug-network.sh` (see `docs/troubleshooting/networking.md`) |
| `quickstart-golden-path.sh` | Validates the documented quickstart without a cluster | `quickstart-validation.yml` |
| `compose_to_stellark8s.py` | Docker Compose to Kubernetes migration helper | See `docs/docker-compose-to-kubernetes-migration.md` |

## Conventions

- Every script documents its usage in a header comment; run `bash <script> --help` where supported.
- Shell scripts must pass `shellcheck -S warning` (`make shellcheck`, pre-commit).
- Pinned tool versions in `setup-*.sh` must stay in sync with `preflight.sh` and the `ci.yml` toolchain.
