# Working in this repository

The product is a **SolarWinds-inspired network operations platform**. Configuration tracking is one subsystem, alongside inventory, monitoring, topology, alerts, traffic analysis, and operational investigation.

## Resume work

1. Read `README.md`, `docs/project-status.md`, and `docs/roadmap.md`.
2. Read the requirements, architecture, data contracts, and decisions relevant to the requested work.
3. Inspect the actual files and working-tree state; documentation can describe planned files that do not exist yet.
4. Follow the user's current request. For implementation, select a task whose predecessors are complete or record why the plan is changing.

## Maintain the project record

- Keep requirement and task IDs stable. The roadmap owns task status and direct predecessors.
- Mark work complete only with the relevant evidence. Distinguish proposed, implemented, and verified behavior.
- Update affected usage, tests, decisions, runbooks, and the current checkpoint with implementation changes.
- Record the exact next task, failed checks, and unresolved decisions so the next session can resume without chat history.
- Use relative links inside repository documentation. Add runnable commands only after the corresponding files exist and the commands have been checked.

## Environment and evidence

- The default test target is the isolated virtual lab described in `docs/lab/virtual-network-lab.md`.
- Public examples and CI artifacts use synthetic material. Read `docs/security.md` before working with capture data or credentials.
- This checkout is under OneDrive. Git ignore rules do not prevent synchronization; operational data belongs in an approved location outside this tree.
- A potential workplace pilot is not authorization to access devices. Its scope and prerequisites are recorded in `docs/operations/production-pilot.md`.

Follow `CONTRIBUTING.md` for the task-to-verification workflow. Do not infer that a planned tool, service, test, Git repository, or release already exists.
