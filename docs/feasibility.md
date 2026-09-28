# Feasibility and risk register

**Assessment date:** 2026-09-28. Documentation research is complete; runtime experiments have not been performed.

The network operations platform is feasible as staged lab work. The highest early uncertainty is whether the selected lab image can expose a realistic, restricted SSH capture path and usable telemetry. Prove those interfaces before building the full application around them.

## Assumptions to validate

| ID | Question | Experiment and evidence | Gate | Current result |
| --- | --- | --- | --- | --- |
| F-01 | Can the host run the lab comfortably? | Record available vCPU/RAM/disk and virtualization support. Run one node, then three; measure resource use and latency. | T-003, T-010 | Unknown |
| F-02 | Is the lab isolated from workplace and household devices? | Review host/VM/container routes, bindings, and firewall policy; test an owned external sentinel is unreachable from lab nodes. | T-003, T-004 | Unverified |
| F-03 | Can a router expose a reliable SSH read interface? | Build one pinned FRR-based image with SSH and an allowlisted command wrapper; compare capture with a console reference. | T-004, T-007 | Unverified |
| F-04 | Can read access avoid arbitrary config writes? | Try a small explicit set of forbidden commands against the lab account; all must fail. Check file permissions and daemon socket access. | T-004 | Unverified |
| F-05 | Can we detect bad output? | Inject pager text, invalid command output, empty capture, and timeout. Record adapter-specific detection limits. | T-007, T-009 | Unverified |
| F-06 | Does the image support the desired metrics? | Add/configure an SNMP agent; verify IF-MIB support, interface mapping, credentials, and unavailable OID behavior. | T-010, T-011 | Unverified |
| F-07 | Can this lab demonstrate topology and flows? | Test LLDP daemon visibility and a software flow exporter with controlled traffic; measure collector footprint. | T-020, T-021 | Unverified |
| F-08 | Can storage survive an interrupted process? | Kill the process at staged write boundaries; restart and verify the baseline and archive. | T-008, T-009 | Unverified |
| F-09 | Are vendor images available if needed? | Choose vendor/model/OS, review its image access terms, and validate an adapter without redistributing proprietary images. | T-031 | Not selected |
| F-10 | Does a workplace pilot solve a real gap? | Network owner confirms existing tool coverage, approved data handling, target list, and success criterion. | T-032 | No authorization or target selected |

## Technical basis and limits

Containerlab supports Linux-based network labs and documents Windows use through WSL. The recommended path here is a dedicated Linux VM for a clearer lifecycle and network boundary. [Installation](https://containerlab.dev/install/), [Windows guidance](https://containerlab.dev/windows/)

FRRouting is suitable for learning routing and configuration behavior; it does not emulate a specific vendor's complete switch hardware or CLI. Containerlab's FRR example demonstrates Linux nodes and virtual links, but its example versions are historical and must not be copied as current pins. [FRR example](https://containerlab.dev/lab-examples/frr01/)

Netmiko supports many device platforms, including Linux. That does not establish a native FRR adapter or guarantee a particular image works unchanged. The proposed lab adapter uses tested SSH behavior and a restricted capture command; a vendor adapter gets its own compatibility evidence. [Netmiko](https://github.com/ktbyers/netmiko), [supported platforms](https://github.com/ktbyers/netmiko/blob/develop/PLATFORMS.md)

## Investigation limits

Spend roughly **3–6 focused hours** on host feasibility and **6–10 hours** on the first lab image. These are planning estimates. If SSH/privilege restrictions are taking longer, use a minimal Linux SSH fixture to prove the collector interface while continuing the real-router investigation. Label fixture-only evidence clearly; it does not prove network-device compatibility.

A host that cannot run the monitoring stack may still support offline development and the one-router milestone. A dedicated machine is the preferred escalation before paid cloud capacity. No paid infrastructure is required by the initial plan.

## Main risks

| Risk | Impact | Response and owner |
| --- | --- | --- |
| Scope grows toward all of SolarWinds at once | Nothing reaches a usable release | Owner keeps v0.1 integrated but small; later capability tracks have explicit gates |
| Lab SSH works only with unrestricted root | Weak security story; unsafe portability | Prove a narrowly allowlisted command path; never describe general shell access as read-only |
| Container network bypasses expected firewall | Lab traffic can reach unintended destinations | Test effective Docker and VM rules; NAT alone is not isolation |
| Real data enters OneDrive or Git | Confidential information can leave the approved boundary | Keep all operational data in an approved unsynced location; scan public artifacts |
| Noisy comparisons | Operators ignore changes | Narrow, versioned normalization; preserve raw data and semantic change tests |
| Emulation differs from work devices | False confidence in compatibility | Maintain a platform/image matrix and require a separate vendor pilot |
| Metrics appear healthy when collection is broken | Failures are hidden | Separate target, exporter, collector, and freshness signals |
| Too many infrastructure services | Resource and maintenance burden | Introduce services only at their milestones and measure their cost |
| Framework/version drift | Instructions stop reproducing | Pin tested versions/digests and refresh the compatibility record per release |

## Decision at this stage

**Proceed with documentation, offline code, and an isolated lab experiment.** Live compatibility, performance, and workplace usefulness remain unproven. Record actual experiment results here when they exist, with tested versions and links to evidence; do not convert a proposed check into a passed result.
