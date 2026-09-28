# Sources and reference adaptation

**Research baseline:** 2026-09-28. External links support product/tool capabilities. Architecture choices, resource allocations, effort estimates, thresholds, and release scope are this project's recommendations and remain subject to implementation evidence.

## How the supplied references were used

The owner's request is the governing scope: create professional documentation for a broad SolarWinds-inspired network operations platform, a virtual lab, and a dependency-driven build process. Instructions embedded in reference documents were treated as source material, not as separate authorization to execute commands, create accounts, publish a repository, or access a network.

| Supplied reference | Adaptation |
| --- | --- |
| `1. Write a one-page project brief.docx` | Problem, users, outcome, success, and constraints in the project brief |
| `2. Check whether the core idea is feasible.docx` | Evidence-driven experiments and unresolved assumptions in feasibility |
| `3. Define your first usable version.docx` | Explicit release scope and verifiable acceptance criteria |
| `4. Sketch how the solution works.docx` | Components, flows, contracts, and consequential design decisions |
| `6. Set up your repo.docx` | README, documentation, source/test boundaries, ignore rules, and eventual clean setup |
| Supplied lifecycle folder's steps 7–9 | Complete vertical slices, requirement-based checks, and reproducible releases |
| Supplied systems-analysis workbook, `Requirements!A1:G101` | Stable requirement ID, category, user, behavior, dependencies, priority, and estimate concepts |
| Workbook `Changes!A1:N101` | Explicit impact of changing scope, dependencies, priority, or requirement wording |
| Workbook `Timeline!A1:DY7` | Dependency-aware sequencing and resource limits; its four-person schedule and numeric estimates were not reused |

The library-occupancy examples were teaching examples, not requirements for this platform. The prior documents in this folder have been rewritten accordingly, including the original configuration-only brief. Requirement/task IDs remain stable when priorities change, avoiding the workbook's need to remap identifiers after renumbering.

The supplied files were read but not modified or copied into the repository. Personal filesystem paths and the workbook's student identifier are intentionally absent from public documentation.

## SolarWinds primary references

| Source | Supports |
| --- | --- |
| [Platform and deployment overview](https://documentation.solarwinds.com/en/success_center/orionplatform/content/core-how-orion-works-sw1625.htm) | Self-Hosted/SaaS distinction and broad capability families |
| [Network Performance Monitor](https://www.solarwinds.com/network-performance-monitor) | Discovery, availability, network performance, topology/path workflows |
| [Network Configuration Manager](https://www.solarwinds.com/network-configuration-manager) | Configuration management capabilities |
| [NCM introduction](https://documentation.solarwinds.com/en/success_center/ncm/content/ncm-introduction.htm) | Device interaction, heterogeneous environments, platform integration |
| [NCM device management](https://www.solarwinds.com/network-configuration-manager/use-cases/network-device-manager) | Archives, policy work, change workflows, firmware functions |
| [NetFlow Traffic Analyzer](https://www.solarwinds.com/netflow-traffic-analyzer) | Flow-based traffic investigation |
| [IP Address Manager](https://www.solarwinds.com/ip-address-manager) | IP address, DHCP, DNS, and conflict workflows |
| [User Device Tracker](https://www.solarwinds.com/user-device-tracker) | Device and switch-port visibility |
| [VoIP & Network Quality Manager](https://www.solarwinds.com/voip-network-quality-manager) | Voice environment and network-quality capabilities |
| [Log Analyzer](https://www.solarwinds.com/log-analyzer) | Log and event investigation |
| [Server & Application Monitor](https://www.solarwinds.com/server-application-monitor) | Adjacent application/infrastructure coverage |
| [Virtualization Manager](https://www.solarwinds.com/virtualization-manager) | Adjacent virtual-infrastructure coverage |

These sources describe vendor capabilities, not a test of those products or a guarantee of any licensed edition. No proprietary implementation or branding is copied.

## Implementation primary references

| Area | Sources |
| --- | --- |
| Lab runtime | [Containerlab installation](https://containerlab.dev/install/), [Windows](https://containerlab.dev/windows/), [FRR example](https://containerlab.dev/lab-examples/frr01/) |
| Router behavior | [FRR basic configuration](https://docs.frrouting.org/en/latest/basic.html), [VTY shell](https://docs.frrouting.org/en/latest/vtysh.html) |
| SSH collection | [Netmiko repository](https://github.com/ktbyers/netmiko), [platform list](https://github.com/ktbyers/netmiko/blob/develop/PLATFORMS.md) |
| Python workflow | [uv](https://docs.astral.sh/uv/), [Ruff](https://docs.astral.sh/ruff/), [FastAPI](https://fastapi.tiangolo.com/) |
| Service runtime and networking | [Docker Compose](https://docs.docker.com/compose/), [Docker packet filtering](https://docs.docker.com/engine/network/packet-filtering-firewalls/) |
| Monitoring | [Prometheus multi-target exporters](https://prometheus.io/docs/guides/multi-target-exporter/), [SNMP exporter](https://github.com/prometheus/snmp_exporter), [blackbox exporter](https://github.com/prometheus/blackbox_exporter) |
| Presentation and alerts | [Grafana provisioning](https://grafana.com/docs/grafana/latest/administration/provisioning/), [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) |
| Topology and flow | [lldpd](https://lldpd.github.io/), [GoFlow2](https://github.com/netsampler/goflow2) |
| Intended inventory | [NetBox documentation](https://netboxlabs.com/docs/netbox/) |
| Infrastructure automation | [Ansible network guide](https://docs.ansible.com/projects/ansible/latest/network/getting_started/), [OpenTofu introduction](https://opentofu.org/docs/v1.7/intro/) |
| Service instrumentation | [OpenTelemetry concepts](https://opentelemetry.io/docs/what-is-opentelemetry/) |
| Optional GitOps | [kind](https://kind.sigs.k8s.io/), [Argo CD](https://argo-cd.readthedocs.io/en/stable/) |
| Delivery security | [GitHub Actions secure-use reference](https://docs.github.com/en/actions/reference/security/secure-use) |

Recheck maintained releases, platform support, image terms, and installation steps before implementation. Where a page uses historical examples or a moving branch, select a supported release deliberately and record the exact version tested.
