# Security and data handling

This platform has access to network management interfaces and potentially sensitive configuration data. Its design must make those boundaries explicit from the first lab collector through any future workplace trial.

**Current status:** security requirements and design only. No access control or isolation behavior has been implemented or verified.

## Data classes

| Class | Examples | Storage and publication rule |
| --- | --- | --- |
| Public synthetic | Invented lab names, configs, topology, screenshots | Commit only after review |
| Runtime secrets | SSH keys/passwords, SNMP auth/privacy values, API credentials | Approved secret mechanism; never Git, image layers, logs, or screenshots |
| Operational confidential | Real configs, inventory, IPs/hostnames, topology, MAC/user associations, logs, flows | Approved private storage and retention; no public GitHub/CI/artifact publication |
| Deployment state | Infrastructure state, database backups, secret references | Private access-controlled storage; review backup and sync behavior |

This project folder is under OneDrive. `.gitignore` only affects Git tracking; it does not prevent sync, backups, indexing, or upload. Place real operational data outside the checkout in an approved unsynced location. A directory named `private` is not itself a security control.

## Threats and controls

| Threat | Required control | Verification |
| --- | --- | --- |
| Collector targets unintended systems | Explicit per-environment IP allowlist; fail closed on unknown targets | Rejected target tests plus network boundary checks |
| Inventory injects commands or filesystem paths | Adapter-selected fixed commands; restricted IDs; canonical storage-root checks | Malicious input fixtures |
| Device impersonation | Strict SSH host-key verification and explicit enrollment | Changed-key test fails |
| Read account can change configuration | Device privilege restrictions or proven lab wrapper; separate bootstrap account | Forbidden-command and filesystem/socket permission tests |
| Sensitive output leaks | Redacted errors, disabled session debug logs, summary-only metrics, artifact review | Sentinel secret tests and manual evidence review |
| Captured files become a public archive | Private storage outside Git/sync; restrictive ownership and retention | Path/permission checks and repository scan |
| Browser/API exposes private objects | Loopback/tunnel for lab, authenticated API, per-device authorization | Unauthorized-object and network-binding tests |
| Exporter becomes an open probing proxy | Internal-only endpoints, allowlisted target generation, egress restrictions | Reject arbitrary destinations and test service reachability |
| Compromised dependency or CI runner reaches the network | Locked dependencies, reviewed/pinned Actions, isolated runners, no operational secrets in public CI | CI permission review and artifact provenance |
| Polling overloads a device | Bounded concurrency, time budgets, explicit intervals and backoff | Load/timeout measurements in lab, separate pilot limit |

## Credential lifecycle

Generate unique lab credentials at deployment time. Enroll SSH fingerprints through a trusted console or validated operator path. Never silently accept a changed fingerprint. Lab teardown/rebuild may legitimately change host keys; verify the replacement and explicitly update trust.

Prefer separate identities for provisioning, collection, monitoring, and human operations. Use SNMPv3 authPriv when the tested target supports it. If a legacy lab profile temporarily uses v2c, label the limitation and keep the community private and the network isolated; do not promote that choice to production by default.

Use environment variables or mounted secret files only through the chosen runtime mechanism; neither makes a secret inherently safe. Avoid command-line secret arguments because they can appear in process listings and histories. For shared or production services, select the organization's approved secret manager.

## Raw configuration handling

The capture archive preserves fidelity. Configurations may contain credentials or sensitive topology; blind redaction before storage could destroy recovery or comparison value. Keep raw captures private, and create explicitly reviewed synthetic/redacted presentation copies for public output. Redaction is not proof that a work configuration is safe to publish.

Encrypt operational storage and backups using the environment's approved controls. Define access, retention, backup destination, and deletion expectations before collecting workplace data. Test a restore without uploading private material to external AI, CI, or documentation services.

## Pilot boundary

Lab permission does not grant workplace permission. Read-only operations still consume device resources and can reveal sensitive data. The [production gate](operations/production-pilot.md) owns the transition: named approver, exact devices, commands, credentials, interval, storage, and stop conditions.

## If a secret is exposed

Stop further publication or collection, revoke/rotate the affected credential through its owner, and identify copies in Git history, artifacts, logs, sync, and backups. Removing a file from the latest commit alone is insufficient. Record a private incident report and publish only non-sensitive corrective lessons where permitted.
