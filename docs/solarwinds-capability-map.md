# SolarWinds capability map

SolarWinds is a product portfolio, not a single switch-backup application. This project uses its network-management workflows as a reference and implements a bounded subset with original code and existing open-source tools.

**Research date:** 2026-09-28. Product naming, packaging, supported devices, and entitlements can change. The mapping below describes documented capabilities; it is not a licensing comparison or a claim of feature parity.

## Product context

SolarWinds Observability Self-Hosted brings together network, infrastructure, and application capabilities on the SolarWinds Platform. SolarWinds also offers Observability SaaS. Deployment, collection, and feature availability differ; a module name in this document does not mean every edition includes it. [SolarWinds platform overview](https://documentation.solarwinds.com/en/success_center/orionplatform/content/core-how-orion-works-sw1625.htm)

Our target is the operational loop: **know the devices → collect observations → retain history → identify changes and failures → explain the evidence**. Automated remediation is a separate future decision.

## Network capability mapping

| Capability and reference | What the operator can do | Project interpretation | Target |
| --- | --- | --- | --- |
| Discovery and inventory — [NPM](https://www.solarwinds.com/network-performance-monitor) | Discover network devices and track their health | Explicit inventory first; later add controlled device discovery in an approved lab range | Inventory v0.1; automatic device discovery extension |
| Availability and performance — [NPM](https://www.solarwinds.com/network-performance-monitor) | Examine availability, latency, loss, interfaces, and supported device metrics | Integrate blackbox and SNMP exporters with Prometheus | v0.1 |
| Topology and path analysis — [NPM](https://www.solarwinds.com/network-performance-monitor) | Visualize relationships and investigate network paths | Declared topology and path probes first; LLDP observations later | Probes v0.1; observed topology v0.3 |
| Configuration archives and comparison — [NCM](https://www.solarwinds.com/network-configuration-manager) | Retain configurations, compare versions, and detect changes | Build Python capture, validation, snapshot, and diff pipeline | v0.1 |
| Scheduled configuration work — [NCM introduction](https://documentation.solarwinds.com/en/success_center/ncm/content/ncm-introduction.htm) | Automate recurring configuration-management tasks | Schedule read-only capture with per-device isolation and run summaries | v0.1 |
| Configuration policy checks — [NCM capabilities](https://www.solarwinds.com/network-configuration-manager/use-cases/network-device-manager) | Evaluate configurations against defined policies | Evaluate a few explicit lab rules with evidence and false-positive cases | v0.2 |
| Change deployment, firmware, and recovery — [NCM capabilities](https://www.solarwinds.com/network-configuration-manager/use-cases/network-device-manager) | Apply changes, distribute firmware, and use archives in recovery workflows | Requires separate authorization and rollback design | Deferred |
| Traffic and bandwidth analysis — [NTA](https://www.solarwinds.com/netflow-traffic-analyzer) | Analyze flow records to investigate traffic patterns | Integrate a supported flow collector, storage, and views after a lab feasibility experiment | v0.3 |
| Address, DHCP, and DNS management — [IPAM](https://www.solarwinds.com/ip-address-manager) | Track address space, conflicts, DHCP, and DNS information | Integrate NetBox for intended inventory and prefixes; DHCP/DNS control remains a separate gap | Inventory/address integration v0.3 |
| Endpoint and switch-port tracking — [UDT](https://www.solarwinds.com/user-device-tracker) | Locate devices and investigate switch-port associations | MAC/ARP/LLDP correlation using synthetic endpoints and supported bridge/port data | v0.3; no user tracking |
| Voice and network quality — [VNQM](https://www.solarwinds.com/voip-network-quality-manager) | Investigate supported voice environments and quality signals such as jitter | Basic path probes only; no call-manager integration or voice-quality equivalence | Deferred |

## Shared operations and adjacent products

| Area | SolarWinds reference | Treatment in this project |
| --- | --- | --- |
| Alerts, dashboards, and reports | [SolarWinds Platform](https://documentation.solarwinds.com/en/success_center/orionplatform/content/core-how-orion-works-sw1625.htm) shared capabilities | Integrate Grafana and Alertmanager; build project-specific health and change context |
| Logs and events | [Log Analyzer](https://www.solarwinds.com/log-analyzer) | Add lab syslog only after metrics and configuration collection are reliable |
| Servers and applications | [Server & Application Monitor](https://www.solarwinds.com/server-application-monitor) | Monitor the lab host and this application; broad application-template coverage is excluded |
| Virtualization | [Virtualization Manager](https://www.solarwinds.com/virtualization-manager) | Observe lab resource consumption; no hypervisor-management replacement |
| Databases, digital experience, incident response, service desk, remote access, and file transfer | Separate areas in the [SolarWinds product portfolio](https://www.solarwinds.com/network-performance-monitor) navigation | Outside this network-focused project; integrate only for a concrete need |
| Advanced analytics and cloud-assisted features | [Platform overview](https://documentation.solarwinds.com/en/success_center/orionplatform/content/core-how-orion-works-sw1625.htm) | Begin with explicit rules and measured behavior; no AI root-cause claims |

## What we build and what we integrate

**Build:** device-adapter contracts, capture validation, immutable history, meaningful diffs, structured results, safe error handling, and eventually a read API and a small policy engine. These are the domain-specific components that make the project more than a dashboard installation.

**Integrate:** network emulation, time-series storage, visualization, alert routing, CI, and infrastructure provisioning. Reimplementing a metrics database or general-purpose dashboard would consume effort without improving the core learning outcome.

## Important distinctions

- A diff reports an observed change between captures. It does not identify the exact edit time or actor.
- Scheduled polling leaves gaps. Syslog-triggered capture can reduce a gap, but event delivery and attribution still require validation.
- SNMP interface counters describe volume and errors. Flow records answer different questions about conversations and traffic distribution.
- A declared topology describes intended wiring. A discovered topology is an observation and may be incomplete or stale.
- A saved running configuration is useful recovery material. Recovery readiness requires an actual restore test, which v0.1 does not include.
- A lab policy result is not certification against a regulatory framework.

The [requirements](requirements.md) and [release gates](roadmap.md) translate this capability map into work we can verify.
