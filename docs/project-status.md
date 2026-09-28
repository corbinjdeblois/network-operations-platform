# Current checkpoint

**Updated:** 2026-09-28  
**Product:** Network Operations Platform  
**Current stage:** initial Git and documentation baseline established; application and lab implementation not started.

## What exists

- A project brief that reflects the broader SolarWinds-style platform scope.
- A sourced capability map, phased requirements, architecture, contracts, and technology decisions.
- A virtual network lab plan with host sizing assumptions, topology, isolation, and validation scenarios.
- A dependency-driven task plan, testing strategy, security model, runbooks, and portfolio evidence plan.
- A GitHub-oriented README and local SVG banner, plus Git ignore rules, contribution guidance, and an agent handoff guide.

The repository is [corbinjdeblois/network-operations-platform](https://github.com/corbinjdeblois/network-operations-platform), with the initial baseline on `main`. This baseline contains documentation and repository configuration. No application source, tests, executable lab, CI workflow, benchmark, production pilot, or release exists.

## Resume here

1. Read [roadmap](roadmap.md). T-002 is in progress; T-003 is ready.
2. T-002 has established the repository name and Git baseline. Continue with Python packaging, dependency locks, offline tests, and CI. The license decision remains open.
3. T-003 confirms lab hardware, hypervisor, addressing, and isolation. Default to a Linux VM unless the owner chooses another host.
4. Proceed through the documented dependencies. The one-router collector milestone is an engineering slice; v0.1 also requires monitoring, dashboards, and alerts.

## Open decisions

| Question | Default for continued planning | Needed by |
| --- | --- | --- |
| Lab machine and available RAM/CPU/disk | Dedicated Linux VM on Windows; proposed sizes in lab plan | T-003 |
| Hypervisor and guest version | Already available supported hypervisor and a supported Ubuntu LTS | T-003 |
| License | Owner decision; no license grant assumed | Public release |
| Weekly time and spending budget | One developer; no mandatory cloud spend or calendar deadline | First implementation checkpoint |
| Vendor fidelity | FRR for initial lab; workplace vendor not selected | T-031 |
| Workplace trial | Optional; no access authorization inferred | T-032 |

## Handoff contract

At the end of each implementation session, update this file with the completed task IDs, evidence references, exact next ready task, unresolved decisions, and any failed checks. Update the task table first; this page should summarize it without becoming a competing backlog.

No background work or automated follow-up is scheduled. Future work starts from this checkpoint when requested.
