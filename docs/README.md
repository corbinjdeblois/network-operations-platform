# Project documentation

This guide connects the product research, implementation plan, and operating model for the Network Operations Platform. Start with the current checkpoint when resuming work. Treat planned behavior as a target until linked evidence demonstrates it.

## Reading paths

| Reader | Suggested path |
| --- | --- |
| Hiring reviewer | [Brief](project-brief.md) → [Architecture](architecture.md) → [Technology choices](technology-strategy.md) → [Demo evidence](portfolio-demo.md) |
| Developer or coding assistant | [Current checkpoint](project-status.md) → [Roadmap](roadmap.md) → [Requirements](requirements.md) → [Tests](testing-strategy.md) |
| Lab operator | [Lab design and setup](lab/virtual-network-lab.md) → [Lab scenarios](lab/test-scenarios.md) → [Operations](operations/runbook.md) |
| Network owner considering a pilot | [Security model](security.md) → [Feasibility](feasibility.md) → [Production gate](operations/production-pilot.md) |

## Documentation map

| Document | Question it answers |
| --- | --- |
| [Project brief](project-brief.md) | What problem are we solving, for whom, and with what boundaries? |
| [SolarWinds capability map](solarwinds-capability-map.md) | What does the reference product family do, and what belongs here? |
| [Feasibility and risks](feasibility.md) | Which assumptions must we prove before investing further? |
| [Requirements](requirements.md) | What observable behavior counts as success? |
| [Architecture](architecture.md) | How do components, trust boundaries, and failures fit together? |
| [Data contracts](data-contracts.md) | What do inventory, snapshots, run results, and future APIs mean? |
| [Technology strategy](technology-strategy.md) | Why these tools, and when do we need them? |
| [Decision log](decisions.md) | What was chosen, what was deferred, and why? |
| [Roadmap and dependencies](roadmap.md) | What can start next, and what does it unblock? |
| [Project status](project-status.md) | What exists, what is unverified, and where do we resume? |
| [Agent handoff guide](../AGENTS.md) | What should a coding assistant read and maintain between sessions? |
| [Virtual network lab](lab/virtual-network-lab.md) | How will we create and isolate a realistic test network? |
| [Lab test scenarios](lab/test-scenarios.md) | How will we demonstrate changes, failures, and recovery? |
| [Testing strategy](testing-strategy.md) | How will requirements become reproducible evidence? |
| [Security and data handling](security.md) | How will credentials, network access, and captured data be controlled? |
| [Operations runbook](operations/runbook.md) | How will we diagnose, recover, and maintain the system? |
| [Production pilot](operations/production-pilot.md) | What must be true before an optional workplace trial? |
| [Portfolio demonstration](portfolio-demo.md) | What should a reviewer be able to reproduce and verify? |
| [Sources and reference adaptation](references.md) | Which claims come from external sources and how were the supplied files used? |

## Sources of truth

- **Requirements** own acceptance criteria and requirement dependencies.
- **Roadmap** owns task IDs, direct task dependencies, estimates, and status.
- **Project status** is a short resumption note, not a second task board.
- **Decision log** records consequential choices. Accepted design does not mean implemented behavior.
- **Evidence** belongs with the code or release that produced it. Private evidence stays in an approved private system.

Use relative links in repository documents so navigation survives cloning and GitHub publication. Diagrams use GitHub-compatible Mermaid. The local SVG banner works without an external image service.

Research baseline: **2026-09-28**. Recheck upstream documentation and compatibility when implementing each milestone.
