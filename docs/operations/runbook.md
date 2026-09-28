# Platform operations runbook

**Status:** operating design. Commands and service names must be added from the tested implementation before the corresponding release. There is no running platform to operate yet.

This runbook helps an operator distinguish a device problem from a platform problem, preserve evidence, and recover without losing history.

## Start and stop contract

**Start:** verify the intended lab/environment, storage mount and free space, runtime secrets, trusted host keys, target allowlist, and isolation. Start network nodes and wait for routing/management readiness. Start storage and monitoring dependencies, then collection, then dashboards and alert routing. Verify fresh data from each expected device.

**Stop:** disable new schedules, allow bounded in-flight jobs to finish or terminate with a recorded failure, stop application services, then stop the named lab if appropriate. Preserve snapshot/database volumes. Stopping is different from destroying a lab or deleting data.

The release implementation must provide exact, tested commands for these steps. Avoid global Docker cleanup commands in routine operation.

## Investigate a failed capture

1. Find the run ID and affected device/environment; inspect the safe error class and most recent successful capture time.
2. Determine whether the collector is healthy and whether failures affect one device or all devices.
3. Check the approved management route, service reachability, target allowlist, and device readiness.
4. For authentication or host-key errors, verify identity and credentials through the approved operator path. Do not disable verification to make the job pass.
5. For command/validation failures, compare the adapter contract with the tested device/image version. Keep the failed capture from becoming a baseline.
6. For storage errors, inspect space, ownership, mounts, staging state, and lock ownership. Preserve successful snapshots.
7. Retry only after the cause is understood. Confirm the new successful capture and normal scheduling.

Do not paste raw configurations into public issue reports. Use synthetic reproduction data and sanitized error metadata.

## Investigate an alert

| Signal | First distinction | Next action |
| --- | --- | --- |
| Device unreachable | Management target failure versus exporter/collector failure | Check `up`, probe result, route, and the affected device |
| Data path failed | Routed path/service failure versus still-healthy management plane | Inspect endpoint route, test service, transit links, and recent changes |
| Interface down | Redundant path available versus outage | Check route convergence and end-to-end probe; retain interface alert context |
| Capture stale | Device healthy but collection absent/failed | Check schedule, last attempt, last success, credentials, and storage |
| Exporter absent | Monitoring cannot observe targets | Restore monitoring; mark affected observations unknown |
| Alert delivery absent | Rule not firing versus routing/receiver failure | Check evaluation state, silence/inhibition, receiver log, and delivery errors |

Acknowledge and silence only through the documented alert workflow. Record duration/reason and avoid indefinite silences for unresolved faults.

## Restore and recover

For file snapshots, restore into an empty approved directory, verify ownership and hashes, and reconcile only complete committed snapshots. Preserve staging files separately for diagnosis if needed; they are not baselines.

For v0.2 database history, restore a backup into an empty disposable database, apply the compatible schema/migration procedure, and verify snapshot references and counts. Reconnect the service only after consistency checks. A backup job's success message is not a restore test.

For a failed application deployment, stop new jobs, revert to the prior reviewed image/configuration, and check schema compatibility before restarting. An irreversible schema change cannot be undone merely by rolling back the container image. Define its migration recovery plan before release.

## Proposed operating targets

These are lab targets to be measured, not existing service-level guarantees:

| Measure | Initial target | Evidence |
| --- | --- | --- |
| Configuration interval | 5 minutes | Scheduler and capture timestamps |
| Stale capture threshold | 15 minutes since last success | Staleness scenario |
| Network scrape interval | 30 seconds | Scrape configuration and samples |
| Reachability alert hold/delivery | 60-second hold; received within 120 seconds of induced failure | Receiver and fault timestamps |
| Healthy three-router capture cycle | At most 60 seconds on the reference host | Timings, config sizes, concurrency |
| Lab restore time | Provisional 30 minutes after dependencies are available | T-029 rehearsal; revise if unsupported |
| Backup recovery point | Proposed daily service-state backup in later operating profile | Backup schedule and restore data age |

Capture interval and backup recovery point describe different losses: missing future observations versus loss of already collected history. Document both in an actual pilot.

## Retention and maintenance

Review disk growth, failed runs, stale devices, expiring credentials, dependency changes, and backup restores. Initial retention proposals are in [data contracts](../data-contracts.md). Before pruning, confirm no sole good baseline is deleted and current investigations retain their evidence.

Update image/tool versions in a disposable lab first. Re-run affected adapter and end-to-end scenarios, record compatibility, then promote the version. Keep changes to the lab image and application independently traceable.

## Incident record

Record start/detection/resolution times, affected components, user impact, evidence, actions, recovery verification, and follow-up tasks. Separate confirmed causes from hypotheses. Include one useful prevention or detection improvement supported by the incident. Public versions use synthetic lab evidence only.
