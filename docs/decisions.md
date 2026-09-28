# Architecture decision log

Accepted decisions are the current design baseline, not completed implementations. Update this log when a consequential choice changes; preserve the old rationale and mark it superseded. All initial entries are dated **2026-09-28**.

## ADR-001 — Build a network operations platform

**Status:** Accepted.  
**Context:** The original folder name and brief focused on configuration tracking. The owner clarified that the product should recreate a broader set of SolarWinds-style workflows.  
**Decision:** Define the product around inventory, monitoring, configuration management, alerts, topology, traffic, and operational investigation. Treat configuration capture as one subsystem and early engineering slice.  
**Alternatives:** A standalone config CLI; attempting full commercial parity in one release.  
**Consequences:** The first usable release integrates several capabilities; later versions remain explicitly bounded. The folder initially retained its earlier name. On 2026-09-28, the owner established `network-operations-platform` as the local folder and GitHub repository name.  
**Revisit:** When changing the product audience or scope.  
**Links:** [Brief](project-brief.md), [capability map](solarwinds-capability-map.md).

## ADR-002 — Start in an isolated Linux lab

**Status:** Accepted as default, host sizing pending.  
**Context:** Workplace access is uncertain, and failure experiments must be reproducible.  
**Decision:** Use a dedicated Linux VM with Containerlab, FRRouting, and synthetic endpoints. Document WSL2 and a dedicated Linux host as alternatives.  
**Alternatives:** Physical hardware, vendor-only emulation, a cloud-first lab.  
**Consequences:** No vendor image purchase is needed for the initial path. Vendor behavior still needs separate validation. Container networking and host privileges require tested isolation.  
**Revisit:** F-01/F-03 fail, hardware changes, or vendor fidelity becomes a requirement.  
**Links:** [Lab plan](lab/virtual-network-lab.md), [feasibility](feasibility.md).

## ADR-003 — Integrate observability infrastructure

**Status:** Accepted.  
**Context:** Time-series storage, dashboards, and alert delivery are substantial systems themselves.  
**Decision:** Use Prometheus, Grafana, and Alertmanager; build project-specific adapters, configuration history, policies, and integration logic.  
**Alternatives:** A custom monitoring stack; a packaged monitoring tool with little original implementation.  
**Consequences:** More initial services than a CLI-only project, but a credible integrated platform. Requires versioned configuration and operational testing.  
**Revisit:** Measured capabilities or resource use no longer fit.  
**Links:** FR-10–FR-12 in [requirements](requirements.md).

## ADR-004 — Keep the application modular

**Status:** Accepted.  
**Context:** CLI, scheduled jobs, and a future API need the same trustworthy capture logic.  
**Decision:** One Python codebase with explicit adapters, services, storage, and presentation boundaries. No microservices or message broker initially.  
**Consequences:** Simpler testing and debugging; network I/O must not block future API request workers. Add a bounded worker path when API-triggered jobs are designed.  
**Revisit:** Independent scaling or isolation is measured to be necessary.  
**Links:** [Architecture](architecture.md).

## ADR-005 — Store snapshots as private immutable files first

**Status:** Accepted.  
**Context:** The earliest critical behavior is reliable history, not complex querying.  
**Decision:** Keep raw and normalized captures plus metadata in a private file store. Use staged writes, atomic publication, locking, and a recoverable baseline pointer. Add PostgreSQL metadata in v0.2.  
**Alternatives:** Git commits for raw configs; database blobs from day one.  
**Consequences:** Public Git never stores operational config history. Filesystem recovery and migration need tests. Retention never deletes the only good baseline.  
**Revisit:** Multiple writers or query requirements exceed the file-store contract.  
**Links:** [Data contracts](data-contracts.md).

## ADR-006 — Keep observation separate from change execution

**Status:** Accepted.  
**Context:** Collection credentials should not also become an automated configuration control plane.  
**Decision:** Collect through fixed read operations with strict host-key checks and allowlisted targets. Lab bootstrap/fault injection uses separate operator privileges.  
**Consequences:** A diff is evidence of a change, not actor attribution, authorization, or a rollback command.  
**Revisit:** A separate device-change feature has an approved workflow, testing, and recovery model.  
**Links:** [Security](security.md), [production gate](operations/production-pilot.md).

## ADR-007 — Preserve intent and observation separately

**Status:** Accepted.  
**Context:** Inventory, discovered topology, and operational measurements can disagree.  
**Decision:** Start with versioned YAML inventory. When NetBox enters, it becomes the intended-inventory authority for that environment. Observations retain their own timestamps and source.  
**Consequences:** Reconciliation flags differences instead of silently overwriting intent. Avoid maintaining two writable authoritative inventories.  
**Revisit:** An agreed operational workflow needs controlled reconciliation writes.  
**Links:** FR-20–FR-23 in [requirements](requirements.md).

## ADR-008 — Deploy simply before adding Kubernetes

**Status:** Accepted.  
**Context:** The project should demonstrate platform engineering without making the network lab depend on cluster complexity.  
**Decision:** Run the first service stack with Compose. Add Ansible/OpenTofu for reproducibility and optional kind/Argo CD for application delivery later.  
**Consequences:** Two deployment targets require a documented scope and shared configuration contracts. Containerlab remains outside Kubernetes.  
**Revisit:** The Compose deployment works and T-028 is selected.  
**Links:** [Technology strategy](technology-strategy.md).

## ADR-009 — Maintain a dependency-driven plan

**Status:** Accepted.  
**Context:** Work must be resumable by the owner and a coding assistant. The supplied workbook demonstrates requirement dependencies and priority-driven scheduling.  
**Decision:** Use stable FR/QR/T IDs, direct predecessors, effort ranges, and evidence-based completion. Keep the canonical task table in Markdown for GitHub review.  
**Consequences:** Reordering never renumbers requirements; no calendar commitments until capacity is known. Documentation changes travel with implementation.  
**Revisit:** Task volume justifies GitHub Projects or an exported spreadsheet, with one authoritative status source.  
**Links:** [Roadmap](roadmap.md), [reference adaptation](references.md).

## Decision template

For a new decision, record ID, date, status, context, alternatives, decision, consequences, reconsideration trigger, and requirement/task links. Use `Proposed`, `Accepted`, `Rejected`, or `Superseded by ADR-XXX`. Reserve decisions for meaningful tradeoffs rather than routine implementation details.
