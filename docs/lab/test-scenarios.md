# Lab test scenarios

These scenarios become the repeatable demonstration and acceptance suite for the platform. **All are currently not run.** Execute only against the isolated lab using separate operator credentials for deliberate changes.

Before each test, record the topology/image versions, start time, relevant configuration, baseline state, and expected outcome. Afterward, restore the known lab state and verify it. Never carry a fault injection over into the next test unintentionally.

| ID | Scenario and action | Expected observation | Recovery | Requirements |
| --- | --- | --- | --- | --- |
| LAB-01 | Deploy from a fresh host profile and generated secrets | Intended nodes, links, routing, private bindings, and allowed/denied paths match the plan | Destroy only the named lab after saving synthetic evidence | FR-13 |
| LAB-02 | Capture one router for the first time, then again unchanged | First result is baseline; second unchanged; both have committed metadata and freshness | None | FR-02–FR-06 |
| LAB-03 | Change a router description through the lab operator interface, then capture | A targeted diff shows the change; raw archive remains intact | Revert description and verify the reverse diff | FR-05, FR-14 |
| LAB-04 | Use wrong credentials and a mismatched host key in separate runs | Distinct failures, no automatic trust bypass, prior good baseline preserved | Restore approved credential reference and verified host key | FR-07, QR-02 |
| LAB-05 | Simulate empty, CLI-error, pager, and truncated capture outputs | Known bad outputs rejected; no false successful baseline | Restore adapter fixture/normal transport | FR-03 |
| LAB-06 | Interrupt writes and deny snapshot-directory access in a disposable copy | No partial snapshot promoted; coherent restart; actionable storage failure | Restore permissions and reconcile complete snapshots | FR-04, FR-15 |
| LAB-07 | Launch overlapping jobs for the same device; fail a different device | Lock prevents corruption; other devices retain independent results | End the extra job and repeat normal schedule | FR-08, FR-09 |
| LAB-08 | Stop R2 so Client B is isolated | Management/device failure and data-path failure appear as appropriate; local receiver gets one grouped alert and later resolution | Start R2, verify routing/service, wait for recovery rule | FR-10–FR-12 |
| LAB-09 | Remove one transit link in the triangle | Interface failure and route reconvergence are visible; data path may remain healthy after convergence | Restore link and verify routing | FR-10, FR-11 |
| LAB-10 | Stop collector/exporter while leaving routers running | Platform failure/freshness warning; no misleading healthy zero; device failure is not inferred solely from exporter loss | Restart service and verify fresh successful observations | FR-09–FR-12 |
| LAB-11 | Silence an expected outage, then end the silence | State is visible while notification behavior follows maintenance policy | Recover device and remove silence | FR-12 |
| LAB-12 | Change a lab route so the test destination becomes unreachable | Config diff, path-failure signal, and alert share a timeline; dashboard does not assert causation automatically | Restore route, compare again, verify recovery | FR-16 |
| LAB-13 | Submit skewed, repeated, and delayed synthetic events | Receipt and source time remain distinguishable; duplicate/uncertain records handled visibly | Reset event fixture | FR-18 |
| LAB-14 | Apply compliant, noncompliant, and unparseable lab policy fixtures | Explicit pass/fail/unknown with rule version and evidence | Restore baseline fixture | FR-19 |
| LAB-15 | Remove an LLDP neighbor and introduce a declared/observed mismatch | Link is stale/missing, not silently deleted from intended topology | Restore link/daemon and re-observe | FR-20 |
| LAB-16 | Generate a bounded transfer between clients through a measured path | Supported flow records show expected endpoints and direction; bytes reconcile within documented tolerance | Stop traffic generator and verify idle/retention behavior | FR-21 |
| LAB-17 | Introduce a NetBox discrepancy and move a synthetic endpoint | Intended state remains unchanged; discrepancy and observed port age/ambiguity are visible | Reconcile lab intent through operator workflow | FR-22, FR-23 |
| LAB-18 | Restore service state into an empty lab deployment | Expected records/hashes present; metrics and capture resume; elapsed recovery measured | Retire disposable restore target after checks | FR-24, QR-07, QR-08 |

## Interpreting the results

A management ping, a routed probe, and an application request test different paths. Label which was measured. An OSPF reroute can keep connectivity alive while an interface is down; the alert model should represent both facts.

LAB-12 creates a known causal relationship because the test operator controls the change. The general product still reports correlated evidence rather than claiming it can infer root cause from timestamps alone.

Performance and traffic tests use bounded rates/durations and synthetic endpoints. Do not use saturation or unrestricted packet generation as a default acceptance test. Record limits in the actual test implementation.
