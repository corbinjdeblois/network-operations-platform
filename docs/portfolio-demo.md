# Portfolio demonstration and evidence

The public project should let a reviewer understand the problem quickly and then inspect the engineering depth. The strongest demonstration is reproducible behavior with clear limits.

**Current evidence:** design documentation only. No implementation, test pass, measured uptime, screenshot, or performance result is claimed.

## First usable platform demonstration

Target a five-to-eight-minute recording after T-015:

1. Explain the platform's purpose and show the lab topology.
2. Show the inventory, healthy network dashboard, and current capture freshness.
3. Capture a baseline and a repeat no-change result. Make an explicitly lab-only configuration edit and inspect its diff.
4. Stop R2 or its test service. Show the relevant device/path signal and notification in the local receiver.
5. Restore the lab and show alert resolution and a fresh successful capture.
6. Demonstrate one failed capture preserving its earlier successful history.
7. Point to the requirement, automated test, architecture decision, and operating step that support the demonstrated behavior.

Do not present a static mockup as a working dashboard. Label synthetic data, deliberate fault injections, and omitted capabilities directly in the recording and screenshots.

## Evidence by release

| Release | Artifacts to publish after verification |
| --- | --- |
| v0.1 | Tested setup/usage, topology, baseline/change/no-change outputs, dashboard screenshot, alert/recovery evidence, test run, measured lab footprint |
| v0.2 | API schema/examples, authorization-negative test, restored metadata, policy result, incident timeline |
| v0.3 | Intended/observed topology difference, bounded traffic comparison, NetBox reconciliation and synthetic endpoint mapping |
| v0.4 | Infrastructure plan, idempotent rebuild, release provenance, restore/rollback result, optional GitOps walkthrough |

Store curated synthetic examples alongside the relevant code/tests. Create `docs/evidence/` only when evidence is available. Include source revision and versions so the results can be reproduced.

## GitHub presentation

Keep the README focused on purpose, current capability, a real demonstration, architecture, and tested setup. Link detailed reasoning from the documentation index. Use CI/release badges only when the linked workflow/release actually exists. Keep future capabilities in the roadmap and current behavior in the usage section.

Before publication, choose the repository name and license, enable the desired security-reporting process, scan the complete candidate history, and review every screenshot/log for private material. Avoid uploading original workplace references or supplied university files into the public repository.

## Interview discussion prompts

- Why use a modular Python core with existing observability tools?
- How does the platform distinguish a device outage from a failed collector?
- What happens if the process dies between writing a snapshot and updating its pointer?
- Why are intended and observed topology stored separately?
- What evidence would justify adding a queue or deploying on Kubernetes?
- Which lab assumptions fail on physical or vendor-specific devices?
- What would be required before a workplace pilot could start?

Use measured outcomes in a future resume summary: supported devices, repeatable scenarios, measured collection/alert times, and recovery evidence. Do not invent percentage improvements or describe a planned platform as deployed at work.
