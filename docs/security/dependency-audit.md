# Dependency Audit

*Addresses issue #1714.*

This document describes how to run the dependency audit for Stellar-K8s,
what each gate checks, and where the results are enforced in CI.

---

## Quick start

```bash
make audit          # full lockfile gate (audit + deny + licenses + locked build)
make audit-quick    # fast gate (audit + deny only, for day-to-day use)
```

Both targets wrap [`scripts/dep-gate.sh`](../../scripts/dep-gate.sh), the single
consolidated lockfile gate. The script relies on
[`.cargo/audit.toml`](../../.cargo/audit.toml) and [`deny.toml`](../../deny.toml)
as the canonical configuration files — advisories are suppressed there with
written justification, never with inline `--ignore` flags.

---

## What the full gate checks

| # | Check | Tool | Fails when |
|---|-------|------|------------|
| 1 | Vulnerability scan | `cargo audit` | An unignored advisory affects `Cargo.lock` |
| 2 | Dependency policy | `cargo deny check` | A license, ban, advisory, or source rule in `deny.toml` is violated |
| 3 | License drift | `generate-third-party-licenses.sh --check` | [`THIRD_PARTY_LICENSES.md`](../../THIRD_PARTY_LICENSES.md) is stale — run `make third-party-licenses` |
| 4 | Lockfile integrity | `cargo build --locked --release` | `Cargo.lock` is inconsistent with `Cargo.toml` — run `cargo update` |

`make audit-quick` runs checks 1–2 only, so dependency-only changes can be
verified in seconds without a full release build.

---

## Configuration files

- [`.cargo/audit.toml`](../../.cargo/audit.toml) — ignored advisories. Every
  entry names the affected transitive crate and why it cannot be remediated
  yet (e.g. upstream ecosystem constraints). Re-evaluate entries when upstream
  publishes a fix.
- [`deny.toml`](../../deny.toml) — license allow-list, banned crates
  (e.g. `openssl` in favour of `rustls`), and advisory/source policy.
- [`.github/dependabot.yml`](../../.github/dependabot.yml) — monthly automated
  update schedule for `cargo`, `github-actions`, and `docker` ecosystems.

---

## CI enforcement

| Workflow | Trigger | What it runs |
|----------|---------|--------------|
| [`ci.yml`](../../.github/workflows/ci.yml) (`security-audit` job) | PR / push touching dependencies | `cargo audit` |
| [`dependency-review.yml`](../../.github/workflows/dependency-review.yml) | Dep changes, weekly schedule | `cargo deny`, stale-dep report, lockfile diff, license inventory |
| [`security-audit.yml`](../../.github/workflows/security-audit.yml) | Daily schedule, manual dispatch | `cargo deny`, `cargo audit`, SBOM upload |
| [`dep-auto-merge.yml`](../../.github/workflows/dep-auto-merge.yml) | Dependabot PRs | Auto-merge when gates pass |

See also [Automated Dependency Updates](dependency-updates.md) for how
Dependabot PRs are identified and merged, and the
[Security Compliance Guide](compliance-guide.md) for the wider policy context.

---

## Adding a new exception

1. Run `make audit` and read which check failed.
2. For a vulnerability: confirm the affected code path is unreachable or
   unexploitable in this project, then add the advisory ID to the `ignore`
   list in `.cargo/audit.toml` with a comment recording the justification
   and a re-evaluation condition.
3. For a license or ban: update `deny.toml` and regenerate the license
   inventory with `make third-party-licenses`.
4. Re-run `make audit` until all four checks pass.
