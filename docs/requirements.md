# Product requirements

**Baseline:** 2026-09-28. All behaviors below are planned. The product is a SolarWinds-inspired network operations platform; configuration tracking is one subsystem.

The first usable platform release, **v0.1.0**, combines inventory, configuration history, health monitoring, dashboards, and alerting for three virtual routers. An earlier one-router capture milestone proves one complete path through the system but is not the whole product.

## Priorities and dependency rules

**Must** means required for v0.1. **Should** means planned in a named later release. **Could** means optional. **Won't in this plan** records an explicit exclusion. Priority is relative to the named release, not a statement that a later capability has little value.

Requirement dependencies below are behavioral prerequisites. Task-level sequencing and effort belong in the [roadmap](roadmap.md). Keep IDs stable when ordering changes. An empty dependency cell is written as `None`, not an Excel error value.

## First platform release

| ID | User need and observable acceptance criterion | Depends on |
| --- | --- | --- |
| FR-01 | **Operator selects approved targets.** Validate explicit device IDs, environment, management IPs, adapter types, and credential references. Reject duplicates, unsafe IDs, unsupported adapters, and targets outside the configured allowlist before opening a connection. | None |
| FR-02 | **Operator captures a configuration.** Retrieve one tested platform's running configuration over SSH using strict host-key verification and a bounded timeout. Capture matches a manual reference after documented transport cleanup. No persistent device change occurs. | FR-01 |
| FR-03 | **Operator can trust capture quality.** Reject empty output, recognized command errors, unsupported privilege responses, and known truncation/pager signatures. Failed validation returns a reason and never promotes a baseline. Completeness is guaranteed only within the tested adapter contract. | FR-02 |
| FR-04 | **Operator retains history.** Save raw capture, comparison text, SHA-256 hashes, UTC time, device/environment identity, and adapter/normalizer version. Publish a successful snapshot only after all required files are durably written. Preserve earlier good snapshots on error. | FR-03 |
| FR-05 | **Operator understands a result.** Return `baseline`, `unchanged`, `changed`, or `failed`; show a deterministic unified diff for a known description/route change. Compare only the same environment, device, config type, and compatible normalization version. | FR-04 |
| FR-06 | **Developer can demonstrate without network access.** Fabricated fixtures exercise baseline, identical input, known change, and invalid capture behavior. Offline mode never opens a network connection. | None |
| FR-07 | **Operator can diagnose failure.** Classify authentication, host-key, timeout, command, validation, storage, and lock failures. Nonzero failure exit status, run ID, and safe metadata are available; credentials and full configs are absent from logs. | FR-02, FR-04 |
| FR-08 | **Operator sees the device inventory.** List three lab routers and their stable ID, role, platform, management endpoint, and last attempt/success. A failure on one router does not suppress results for the others. | FR-01, FR-07 |
| FR-09 | **Operator receives scheduled captures.** Run every five minutes by default in the lab. Enforce a per-device lock, timeout, and bounded transient retry; overlapping jobs do not corrupt state. Record skipped and missed work. | FR-04, FR-08 |
| FR-10 | **Operator sees network health.** Collect ICMP or equivalent reachability plus tested SNMP interface status/counters from the lab. Report collector health separately. Unsupported OIDs are unavailable, not zero. | FR-08 |
| FR-11 | **Operator sees an operational dashboard.** Show reachability, interface status/utilization where link speed is valid, capture freshness, last change, and failures. Provision dashboards from versioned files. Stale/unknown states are visible in text. | FR-09, FR-10 |
| FR-12 | **Operator receives actionable alerts.** A lab outage triggers one deduplicated notification in a local test receiver after a configured hold period; recovery resolves it. Maintenance silence, collector failure, and stale capture alerts are tested. | FR-09, FR-10 |
| FR-13 | **Developer recreates the environment.** Build three routers and two endpoints from a clean lab host using versioned topology, image references, and configuration templates. Recreate generated credentials locally. Prove isolation and controlled teardown. | None |
| FR-14 | **Operator preserves capture fidelity.** Retain raw bytes privately. Apply only documented transport cleanup and narrow versioned comparison rules. A semantic route or interface change remains visible. A ruleset change requires an explicit new comparison baseline. | FR-04, FR-05 |
| FR-15 | **Operator can resume after interruption.** Interrupt before and after snapshot publication. On restart, ignore incomplete staging data, retain committed snapshots, and recover a coherent baseline under the per-device lock. | FR-04, FR-09 |
| FR-16 | **Reviewer reproduces a complete workflow.** Follow clean setup instructions to observe healthy devices, capture a known change, trigger a lab failure, receive an alert, and recover. Record versions, inputs, expected results, and actual evidence. | FR-06, FR-11, FR-12, FR-13, FR-14, FR-15 |

FR-01 through FR-16 are **Must for v0.1.0**. Raw configurations and diffs stay local/private. Dashboard summaries do not contain their text.

## Expansion requirements

| ID | Release and priority | Acceptance criterion | Depends on |
| --- | --- | --- | --- |
| FR-17 | v0.2 Should | A versioned, authenticated read API returns paginated device/run/snapshot metadata; authorized diff access uses explicit device scope. Unknown IDs, denied access, and dependency failures have documented responses. | FR-08, FR-14 |
| FR-18 | v0.2 Should | Receive lab syslog, normalize event timestamps, preserve receipt time, and correlate with capture intervals using device identity. Label uncertain ordering; do not infer causation or actor identity from a diff. | FR-17 |
| FR-19 | v0.2 Should | Evaluate three explicit lab configuration rules. Each result includes rule version, pass/fail/unknown, evidence location, and tests for compliant, noncompliant, and unparseable input. | FR-14, FR-17 |
| FR-20 | v0.3 Should | Collect LLDP from supported lab nodes; display observed links with last-seen time. Keep intended and observed links distinct; missing neighbors become unknown/stale. | FR-08, FR-11, FR-13 |
| FR-21 | v0.3 Should | Generate controlled traffic, export a supported flow protocol, ingest records, and display top endpoints/conversations. Reconcile observed bytes against the traffic test within a documented tolerance and account for sampling/timeouts. | FR-10, FR-11, FR-13 |
| FR-22 | v0.3 Should | Integrate NetBox lab inventory and prefixes through a read-only adapter. Detect inventory/address discrepancies without overwriting intended state. Document that DHCP/DNS lifecycle control remains out of scope. | FR-08, FR-17 |
| FR-23 | v0.3 Should | Map synthetic endpoint MAC/IP observations to supported lab bridge/port data. Show observation age and ambiguity. No workplace user identities or automated port shutdown. | FR-20, FR-22 |
| FR-24 | v0.4 Should | Provision the chosen host through supported infrastructure tooling and configure services reproducibly. Re-running converges without losing retained data; rollback and restore work. | FR-16, FR-17 |
| FR-25 | v0.4 Could | Deploy the application to a local Kubernetes test cluster through GitOps. Demonstrate health checks, restricted network paths, persistent storage, rollout, and rollback; keep network emulation outside the cluster. | FR-24 |
| FR-26 | Optional pilot Could | Validate a specifically approved workplace device adapter, run a small read-only canary, measure impact, and complete the production gate. Lab success alone does not satisfy this requirement. | FR-16, FR-24 |

## Quality requirements

| ID | Applies from | Acceptance criterion |
| --- | --- | --- |
| QR-01 | v0.1 | Clean environment setup uses documented Python/tool versions and a committed dependency lockfile. Offline tests need no network credentials. |
| QR-02 | v0.1 | SSH host-key mismatch fails closed. Secrets are referenced, never committed; synthetic sentinel secrets do not appear in logs, summaries, or CI artifacts. |
| QR-03 | v0.1 | A failed or interrupted run never destroys a successful snapshot. Paths cannot escape the configured storage root; device IDs cannot inject commands. |
| QR-04 | v0.1 | For the reference three-router lab, a healthy capture cycle completes within 60 seconds with configured concurrency at most three. Record hardware, config sizes, and timings; this is an unverified target, not a benchmark. |
| QR-05 | v0.1 | Metrics scrape every 30 seconds in the reference lab. Reachability alerts use a 60-second hold and arrive within 120 seconds in the test receiver. A capture becomes stale after 15 minutes; missing results are never healthy zeroes. |
| QR-06 | v0.1 | Each accepted requirement links to evidence. Dashboards use readable labels and status words, not color alone. Timestamps identify UTC and collection age. |
| QR-07 | v0.2 | API authorization has negative tests; logs include correlation IDs without configuration content. Database backup is restored into an empty environment and checked. |
| QR-08 | v0.4 | A release records source revision, image digest, dependency inventory, vulnerability review, deployment configuration, and measured recovery outcomes. |

Polling/alert values are initial lab settings and may change through a documented decision. Never copy these values directly into a workplace network without its capacity and access review.

## Excluded from the present plan

- Full SolarWinds product parity, enterprise-scale/high-availability claims, and commercial support guarantees.
- Unattended device changes, firmware deployment, automatic config restore, or automated switch-port shutdown.
- Full DHCP/DNS administration, telephony management, deep packet inspection, and a general IT service desk.
- Personal activity tracking, automatic root-cause claims, and regulatory certification.

These exclusions bound the implementation sequence; they do not reduce the project to configuration tracking. The [capability map](solarwinds-capability-map.md) remains the broader product reference.
