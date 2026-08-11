---
name: cmsis-pack-topology-evidence
description: Collect and review evidence-backed CMSIS-Pack debug and trace topology for an existing Device Family Pack PDSC. Use before debug-description, non-trace debug-sequence, debug-setup, or CoreSight trace-sequence work whenever connection, DP/AP, CoreSight, dormant-state, or available trace-path facts must be established or refreshed. Do not edit PDSC debug descriptions or sequences.
---

# CMSIS-Pack topology evidence

Collect the shared debug and trace topology record. This skill owns evidence gathering and its readiness state; downstream skills consume the record as read-only input.

## Entry and progressive workflow

1. Find the target `.pdsc`, selected family/subFamily/device/variant scope, affected `<processor>` descriptions, and existing `.agent-artifacts/<pdsc-stem>.debug-topology.md`. Do not create a new DFP and do not continue if no device description exists.
2. Before gathering evidence, read `references/evidence-contract.md`.
3. Load only the target PDSC, selected device subtree, and evidence for the requested connection and every supported trace path. Do not load non-trace sequence records or trace component assets.

## Consumer skills

- `$cmsis-pack-debug-description` uses verified topology to create non-sequence PDSC definitions.
- `$cmsis-pack-debug-sequences` uses it for device-specific non-trace behavior.
- `$cmsis-pack-trace-sequences` uses it only when the record is `READY` and contains evidence for every available trace path.
- `$cmsis-pack-debug-setup` calls this skill first when coordinating multiple specialists.
