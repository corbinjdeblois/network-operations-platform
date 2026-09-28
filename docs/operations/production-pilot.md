# Optional production pilot gate

The platform can be developed and demonstrated entirely in the virtual lab. A workplace test is a later, optional validation step; nothing in this plan authorizes access to work devices or collection of their data.

**Current status:** not requested from a network owner, not approved, and not ready. No production target or credentials have been selected. T-031/T-032 remain optional.

## Entry criteria

| Gate | Required record |
| --- | --- |
| Network-owner authorization | Named owner, purpose, exact target devices, allowed commands/protocols, collection window and frequency |
| Confirmed value | Existing monitoring/config-backup coverage and the specific gap the pilot evaluates |
| Tested adapter | Vendor/model/OS, command semantics, privilege requirements, output validation, manual comparison, and negative tests |
| Limited account | Dedicated read identity, minimum privileges, strict host-key enrollment, approved credential handling |
| Bounded workload | One initial device, conservative interval, concurrency one, timeouts/backoff, measured device-resource budget |
| Approved data path | Collector location, explicit routes/ports, storage outside unapproved sync, encryption/access, retention and deletion |
| Failure readiness | Lab acceptance complete, normal tooling left in place, collector stop procedure, recovery contacts and escalation |
| Publication boundary | What may be documented publicly; default excludes all real configuration, inventory, topology, screenshots, and events |

Record approvals in the workplace's approved system. Keep only a non-sensitive reference in the public project if permitted. Do not put authorization records containing internal identifiers into GitHub.

## Canary sequence

1. Choose one low-risk approved device and establish baseline CPU, memory, control-plane health, and existing tool coverage.
2. Verify host key and execute the approved read operation once during the agreed window. Compare output privately with a manual reference.
3. Inspect device load, session behavior, collection completeness, storage placement, and absence of persistent device changes.
4. Run a small, explicitly agreed number of scheduled captures. Measure reliability and duration; stop automatically on the pre-agreed limits.
5. Review results with the network owner. Expand only through a documented decision and a new bounded target list.

Do not inject outages, route changes, traffic floods, bad credentials, or other fault scenarios into production as part of this pilot. Those tests belong in the lab or a separately approved non-production environment.

## Stop conditions

Stop collection if it affects device responsiveness, exceeds the agreed load/session limits, reaches an unintended target, cannot validate the device identity, exposes data outside the approved boundary, or produces unexplained command behavior. The initial pilot's response is to stop the collector and preserve approved evidence, not to change device configuration automatically.

## Exit report

Record the authorized scope, exact platform versions, number of attempts/successes/failures, measured duration and impact, detected limitations, private data retention/deletion action, and the owner's decision: stop, continue the canary, or authorize a bounded expansion.

A successful canary validates that device and scenario. It does not establish enterprise readiness, multi-vendor support, or permission for further networks.
