# Technology strategy

The aim is to demonstrate how to build and operate a network platform. Tool selection is based on a concrete responsibility, repeatability, testability, and manageable local resource use. These are project recommendations, not a claim about employer demand rankings.

**Status:** design choices only. Select and record exact supported versions during implementation. Commit a Python lockfile, pin container images by digest after validation, and document compatibility instead of claiming untested `latest` images work.

## Core stack

| Tool | Role and reason | Enters | Tradeoff |
| --- | --- | --- | --- |
| Python | Collection, validation, diff logic, orchestration, and later API integration | Foundation | Keep network I/O bounded; test adapters against real lab behavior |
| [uv](https://docs.astral.sh/uv/) | Project environment, dependency resolution, and lockfile | T-002 | Contributors need the documented tool version and frozen install path |
| [Netmiko](https://github.com/ktbyers/netmiko) | SSH interaction with supported device types | T-007 | Broad library support does not prove this project's adapter support |
| pytest and [Ruff](https://docs.astral.sh/ruff/) | Behavioral tests, linting, and formatting | T-002 onward | Prefer meaningful contracts/failure tests over a coverage percentage target |
| Linux VM | Stable home for containers, namespaces, routing, and service operations | T-003 | Requires virtualization and measured capacity |
| [Containerlab](https://containerlab.dev/install/) | Declarative network topology and container-node lifecycle | T-004 | Host networking privileges require a dedicated lab boundary |
| [FRRouting](https://docs.frrouting.org/en/latest/basic.html) | Open-source router behavior and configuration learning | T-004 | Does not reproduce vendor ASICs or every vendor CLI |
| [Docker Compose](https://docs.docker.com/compose/) | Run monitoring dependencies as one local service stack | T-011 | Container network/firewall behavior must be tested explicitly |
| [Prometheus exporters](https://prometheus.io/docs/guides/multi-target-exporter/) | Poll reachability and SNMP, store time-series observations | T-011 | Counter resets, missing data, and label cardinality need deliberate handling |
| [Grafana provisioning](https://grafana.com/docs/grafana/latest/administration/provisioning/) | Versioned dashboards and data sources | T-013 | UI edits must be exported back to source to survive provisioning |
| [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) | Grouping, deduplication, silences, and notification routing | T-014 | Start with a local receiver; test behavior before external routing |
| [GitHub Actions](https://docs.github.com/en/actions/reference/security/secure-use) | Reproducible pull-request checks and release workflows | T-002 onward | Pin third-party actions and keep public runners away from operational secrets |

pytest is a planned testing choice; its exact supported version is resolved with the rest of the Python toolchain. No packages or runtime services are installed by this documentation work.

## Expansion stack

| Tool or approach | Role | Adoption gate |
| --- | --- | --- |
| [FastAPI](https://fastapi.tiangolo.com/) and PostgreSQL | Typed service contract and relational metadata/history | T-016, after the integrated lab is working |
| Structured JSON logs; later [OpenTelemetry](https://opentelemetry.io/docs/what-is-opentelemetry/) | Correlate application work and diagnose service failures | Add tracing when there is a service path worth tracing; OTel is instrumentation, not a metrics database |
| [lldpd](https://lldpd.github.io/) or supported device LLDP interface | Observed neighbor collection | T-020, with tests proving actual lab-link visibility |
| [GoFlow2](https://github.com/netsampler/goflow2) | Candidate protocol decoder/collector for flow ingest | T-021 spike must also choose exporter, storage, aggregation, and presentation; GoFlow2 alone supplies none of the latter |
| [NetBox](https://netboxlabs.com/docs/netbox/) | Intended device inventory, prefixes, and address data | T-023, when reconciling intent and observations is useful |
| [Ansible](https://docs.ansible.com/projects/ansible/latest/network/getting_started/) | Repeatable Linux host/service configuration and lab bootstrap | T-026; keep bootstrap privileges separate from collection credentials |
| [OpenTofu](https://opentofu.org/docs/v1.7/intro/) | Declarative provisioning for the chosen host/provider | T-026 only where a supported provider fits; a local VM may begin with documented native setup |
| [kind](https://kind.sigs.k8s.io/) and [Argo CD](https://argo-cd.readthedocs.io/en/stable/) | Local Kubernetes deployment and GitOps reconciliation practice | Optional T-028 after a tested container service exists |

OpenTofu teaches the infrastructure-as-code workflow associated with the Terraform ecosystem. If a target employer or selected provider specifically needs Terraform, record the reason and compatibility implications before switching. Do not maintain two equivalent infrastructure implementations for appearance alone.

## Platform engineering evidence

| Practice | What the project should prove |
| --- | --- |
| Reproducibility | A fresh machine can recreate the lab and application using recorded inputs |
| Developer experience | One documented path to install, test, run fixtures, and inspect failures |
| Service design | Stable contracts, bounded failures, explicit ownership, and compatible migrations |
| Delivery | Reviewed changes, isolated CI, traceable releases, and recoverable deployments |
| Observability | Signals answer operator questions and expose the platform's own failure modes |
| Security | Restricted collectors, private data boundaries, trusted dependencies, and tested access control |
| Operations | A second person can diagnose an outage and restore the service from the runbook |

## Deliberate exclusions at the start

A custom frontend, service mesh, Kafka cluster, multi-cloud deployment, and Kubernetes operators are not prerequisites for the first release. Introduce one only when the requirements and measurements justify its cost. A large tool list is less persuasive than an integrated system with demonstrable behavior.
