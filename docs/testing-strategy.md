# Testing and acceptance strategy

Tests should show that the platform behaves correctly when the network, collector, storage, or an input fails. Successful startup alone is not a release criterion.

**Current result:** no application tests or lab scenarios have run. This document defines the verification plan. The documentation baseline receives link, consistency, and diagram review separately.

## Test layers

| Layer | Purpose | Where it runs |
| --- | --- | --- |
| Unit | Inventory validation, normalization, diff behavior, identity, status mapping, policy rules | Local development and public CI, synthetic data only |
| Component | Snapshot transactions, process interruption, locking, storage failures, fake SSH outcomes | Local/CI temporary directories and controlled fixtures |
| Integration | Actual SSH capture, restricted account, SNMP/exporter behavior, dashboard provisioning, API/database | Disposable isolated lab |
| End to end | Healthy platform → known change → outage → alert → recovery | Reference three-router lab |
| Operational | Restore, retention, deployment rollback, resource limits, stale/missing data | Disposable copy of the lab service state |
| Optional vendor/pilot | Specific model/OS compatibility and bounded impact | Approved non-production device, then optional workplace canary |

A mock proves the application handles a modeled response. It does not prove that a device supports a command, emits complete output, or enforces permissions.

## Requirement traceability

| Requirement group | Primary evidence | Owning task |
| --- | --- | --- |
| FR-01, FR-08 | Duplicate/invalid/unsafe identity and target tests; independent multi-device outcomes | T-006, T-012 |
| FR-02, FR-03, FR-07 | Manual capture comparison, host-key/auth/timeout/command/truncation tests | T-007, T-009 |
| FR-04, FR-14, FR-15 | Raw/normalized hashes, semantic-diff fixtures, interrupted-publication and restart recovery | T-008, T-009, T-012 |
| FR-05, FR-06 | Baseline/unchanged/changed/failed fixture outputs; offline mode proves no network calls | T-005, T-009 |
| FR-09 | Overlap lock, concurrency budget, scheduler restart and skipped-job records | T-012 |
| FR-10, FR-11 | Counter behavior, unsupported OIDs, collector failure, stale-data and dashboard inspection | T-011, T-013 |
| FR-12 | Hold time, delivery latency, deduplication, silence and resolution in local receiver | T-014 |
| FR-13, FR-16 | Fresh rebuild and recorded integrated demonstration | T-010, T-015 |
| FR-17, QR-07 | API contract/authorization, object-scope denial, migration, empty-destination restore | T-016, T-019 |
| FR-18, FR-19 | Clock-skew/event fixtures and policy pass/fail/unknown cases | T-017, T-018 |
| FR-20–FR-23 | Intended/observed topology, controlled traffic, inventory discrepancy, endpoint ambiguity | T-020–T-025 |
| FR-24, QR-08 | Idempotent setup, traceable release, restore and rollback | T-026, T-027, T-029, T-030 |
| FR-25, FR-26 | Optional GitOps or vendor/pilot evidence only when selected | T-028, T-031, T-032 |

QR-01–QR-03 are continuous checks from the foundation. QR-04–QR-06 are measured and reviewed at T-015. Later requirements do not retroactively become necessary for v0.1.

## Required adversarial cases

- Empty inventory; duplicate device IDs; path traversal; malicious/unsupported command selection; address outside allowlist.
- Host-key mismatch; missing key; bad credentials; unreachable host; refused connection; timeout after partial output.
- Empty capture; pager residue; recognized CLI error; unexpectedly small/incomplete output within known adapter signatures.
- Disk full; permission denied; a process killed at each publication boundary; two simultaneous captures for one device.
- Different device/environment/config type; incompatible normalization versions; semantic changes that broad cleanup could conceal.
- Missing SNMP object; counter reset; unknown interface speed; exporter alive but target unavailable; absent metric series.
- Scheduler stopped while devices stay healthy; delayed recovery; maintenance silence; duplicate alert events.
- Later API unauthorized object IDs, oversize queries, inaccessible storage, and database migration/restore errors.

Use synthetic credentials with recognizable sentinel values and assert they never appear in logs, exceptions, metrics, or public test artifacts. For snapshot content, preserve the raw private data contract; the test is about preventing accidental public/log disclosure, not deleting secrets from an operational archive without a policy.

## Reference measurements

For T-015, record actual host/guest capacity, software versions, image digests, router count, config sizes, collection interval, concurrency, scrape interval, and retention. Measure healthy cycle duration, peak memory/disk, time from induced failure to received alert, and time to resolution.

Compare these measurements to QR-04/QR-05. If targets fail, report the failure and either fix the system or update the target with a justified decision before release. Never replace absent measurements with an estimate and label it a result.

## CI boundaries

Public CI runs offline fixtures and software tests. It must not connect to work devices or receive operational credentials. Docker/network integration tests may use a disposable isolated runner only after its privilege and isolation model is documented. Untrusted pull requests must not run on a persistent self-hosted runner that can reach the lab or production network.

Pin third-party Actions to reviewed full commit SHAs, grant minimal token permissions, and keep forked-PR checks free of deployment secrets. [GitHub secure-use reference](https://docs.github.com/en/actions/reference/security/secure-use)

## Evidence format

Every acceptance record includes requirement and task IDs, source revision, environment/image versions, inputs, action, expected outcome, actual outcome, and evidence location. Mark each check **Passed**, **Failed**, **Not run**, or **Not applicable with reason**. Screenshots supplement reproducible commands and structured results; they do not replace them.

Only synthetic evidence is eligible for `docs/evidence/` in the public repository. That directory will be created when evidence exists. Operational evidence belongs in an approved private record, with a non-sensitive reference in the plan if permitted.
