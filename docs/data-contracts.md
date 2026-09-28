# Data contracts

**Status:** proposed contracts for implementation. Examples use invented lab identifiers and values. Fields and commands are not yet implemented.

## Identity and inventory

A stable identity is `(environment, device_id)`. IP address and hostname may change; neither alone is a safe history key. Permit only a restricted device-ID pattern such as lowercase letters, digits, and hyphens, with a documented maximum length. Reject path separators, shell syntax, duplicates, and unsupported adapter names.

Illustrative inventory entry:

```yaml
schema_version: 1
environment: lab
allowed_management_cidrs:
  - 172.31.250.0/24
devices:
  - device_id: r1
    role: core
    management_ip: 172.31.250.11
    adapter: frr_lab_ssh
    credential_ref: lab-router-read
    config_type: running
```

`frr_lab_ssh` is this project's planned adapter identifier, not a claim that Netmiko exposes a driver with that name. The adapter decides the underlying transport type and fixed command. Inventory never supplies arbitrary commands or passwords. All resolved addresses must pass the environment allowlist; the first implementation can accept literal IPs only to simplify this boundary.

## Capture result

A run has an opaque `run_id`, environment, start/end UTC timestamps, attempted-device count, and per-device results. One failed device makes the aggregate run partial/failed without discarding other results.

| Field | Meaning |
| --- | --- |
| `schema_version` | Version of the result contract |
| `device_id`, `environment` | Stable history scope |
| `status` | `baseline`, `unchanged`, `changed`, or `failed` |
| `snapshot_id` | Committed snapshot identity, absent on failure |
| `previous_snapshot_id` | Prior compatible successful snapshot, if any |
| `started_at`, `completed_at` | UTC timestamps with explicit timezone |
| `duration_seconds` | Measured elapsed time, preferably from a monotonic timer |
| `error_code` | Safe classified reason, absent on success |
| `adapter_version`, `normalizer_version` | Interpretation provenance |
| `raw_sha256`, `normalized_sha256` | Integrity hashes, present only for committed payloads |

Proposed CLI exit contract: `0` for successful collection including detected changes, `1` for any device/runtime failure, `2` for invalid invocation/inventory before collection. A configuration change is a normal result, not an execution error. A separate future policy-check command may define a distinct noncompliance exit status.

## Snapshot layout

Illustrative paths under an approved private root:

```text
<private-root>/lab/r1/running/
  snapshots/<snapshot-id>/
    raw.cfg
    normalized.cfg
    metadata.json
  latest-success.json
  staging/<run-id>/
  runs/<run-id>.json
```

`<private-root>` is outside the public checkout and outside unauthorized sync locations. Do not copy this literal placeholder into configuration. Restrict ownership and permissions to the service account and approved operators.

A snapshot ID is collision resistant and filesystem safe. Do not depend on second-resolution timestamps alone. Capture metadata records hash values, command identifier, software/image version where known, byte counts, validation outcome, and normalization provenance. Never store credentials in metadata.

## Baseline and normalization rules

1. Acquire the per-device/config-type lock and find the latest committed, compatible success.
2. Preserve the exact received configuration payload after defining where transport framing ends. Keep any transport-session debug output disabled by default.
3. Derive comparison text with explicit line-ending, prompt, and paging cleanup rules tested for the adapter.
4. Do not sort all lines or broadly remove timestamps/numbers: ordering and numeric values may be semantic.
5. Store raw and normalized hashes and the normalizer version. Save successful unchanged captures too, so freshness has evidence.
6. A first capture has no diff baseline. A normalizer-version change starts a labelled new comparison baseline unless an explicit migration reprocesses both raw versions.
7. Only complete committed snapshot directories are eligible. Reconcile an out-of-date pointer on restart; never promote staging content.

Detecting all possible truncation is not generally achievable from arbitrary text. The contract covers documented failure signatures and the tested device command. Record known limits per adapter.

## Telemetry model

Proposed application metric families include capture attempts/failures, capture duration, last-attempt time, last-success time, and last-change time. Use bounded labels such as environment, device ID, adapter, and error class. Never use complete messages, run IDs, configuration lines, MAC addresses, or arbitrary endpoint pairs as labels.

For interface traffic, derive rates from counters and handle resets. Prefer 64-bit counters where available. Utilization requires a known valid interface speed; an emulated interface may not have a meaningful physical capacity. Report unavailable utilization rather than an invented percentage.

A change time means the capture time at which a difference was first observed. Its actual edit time lies between compatible observations unless another trusted event narrows the interval.

## API direction for v0.2

Design targets, not live endpoints:

| Method and path | Intended response |
| --- | --- |
| `GET /api/v1/devices` | Paginated allowed devices and freshness metadata |
| `GET /api/v1/devices/{id}/captures` | Capture metadata ordered by time with stable pagination |
| `GET /api/v1/devices/{id}/diffs/{snapshot_id}` | Authorized diff for the stated device and compatible predecessor |
| `GET /api/v1/runs/{run_id}` | Run outcome with per-device results |
| `GET /health/live` | Process liveness without private details |
| `GET /health/ready` | Ability to serve using required dependencies |

The API must validate environment/device ownership for every object reference. Raw payload access is excluded by default. No endpoint accepts arbitrary shell commands or open-ended scan destinations. Define authentication, authorization, pagination bounds, request limits, and 400/401/403/404/503 behavior before implementing a shared service.

## Retention and migration

Initial lab defaults: successful snapshots for 30 days while retaining at least one good baseline per active device, metrics for 7 days, and application logs for 7 days. These are proposed capacity settings, not workplace policy. Flow retention is selected after its volume experiment.

Before enabling pruning, test boundary dates, protected baselines, failed deletions, and restart behavior. Before adding PostgreSQL, implement a repeatable file-to-metadata import with checksums and duplicate handling. A migration or backup is complete only after a verified restore/import into an empty destination.
