# Project brief

**Project:** Network Operations Platform  
**Owner:** Corbin de Blois  
**Repository:** [network-operations-platform](https://github.com/corbinjdeblois/network-operations-platform)  
**Status:** design baseline, 2026-09-28

## Purpose

Build a SolarWinds-inspired network operations platform and a reproducible virtual network on which to develop and test it. The platform should help an operator understand network inventory and topology, monitor health, inspect configuration changes, analyze traffic, and investigate failures.

Configuration tracking is one subsystem. The overall project is an integrated network-management and observability platform, developed in stages and supported by professional engineering documentation.

## Problem and users

Network investigation crosses several kinds of evidence: devices and links, health metrics, configurations, events, and traffic. When these are disconnected, it takes longer to understand the state of a network and assess a possible cause of failure.

The first user is the developer operating the virtual lab. A second user is an engineer reproducing the project from GitHub. A workplace network team is a possible later user if the platform fills a confirmed gap and its owner authorizes a controlled trial.

## Product vision

The operator should be able to move from a network overview to a device, see its current and recent health, inspect configuration history, and relate those observations to alerts and events. Later releases add observed topology, flow analysis, intended address/inventory data, and endpoint-to-port visibility.

The project will build domain-specific collection, validation, history, and integration logic while using established infrastructure for storage, dashboards, alerts, and delivery. The [SolarWinds capability map](solarwinds-capability-map.md) defines the reference scope and known gaps.

## First usable platform

Version 0.1.0 operates a three-router virtual network with two synthetic endpoints. It includes explicit device inventory, scheduled read-only configuration capture and comparison, reachability/interface metrics, an operational dashboard, and local test alerts for failures and stale collection.

Before that release, an engineering milestone proves one router's capture-to-diff path. This milestone reduces integration risk; it is not the product's final scope. See [requirements](requirements.md) and the [dependency roadmap](roadmap.md).

## Success criteria

- A second engineer can rebuild the lab and reproduce a healthy network, a known configuration change, an outage, an alert, and recovery.
- Failed collection preserves prior good history and makes stale or unavailable observations explicit.
- The platform separates observed facts from intended inventory and avoids claiming causation from timing alone.
- Each release has traceable requirements, meaningful tests, versioned deployment inputs, and an operating runbook.
- The public repository tells a truthful engineering story through source, decisions, measured results, and reproducible examples.

## Constraints and boundaries

Assume one developer, a local Linux VM, open-source lab components, and no required cloud spend. Host capacity, weekly availability, and the first workplace device vendor are unconfirmed. Estimates are provisional until the lab is measured.

The current sequence excludes full commercial feature parity, enterprise-scale guarantees, personal activity tracking, automatic remediation, and device firmware/configuration deployment. Workplace access and publication of operational data require a separate decision. All public demonstrations use fabricated lab material.

## Platform engineering outcomes

Demonstrate Linux networking, Python service design, CI, infrastructure as code, observability, secure data handling, failure testing, release discipline, and recovery. Choose tools because they support these outcomes. Kubernetes is an optional later delivery target after the platform works in a simpler environment.

## Immediate next step

Continue **T-002** and start **T-003**: complete the Python development baseline and verify the lab host. The Git repository and documentation baseline are established. Then build one router's management interface and the first collection path. Resume from [project status](project-status.md).
