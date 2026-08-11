---
name: cmsis-pack-topology-evidence
description: Collect and review evidence-backed CMSIS-Pack debug and trace topology for an existing Device Family Pack PDSC. Use before debug-description, non-trace debug-sequence, debug-setup, or CoreSight trace-sequence work whenever connection, DP/AP, CoreSight, dormant-state, or trace-path facts must be established or refreshed. Do not edit PDSC debug descriptions or sequences.
---

# CMSIS-Pack topology evidence

Collect the shared debug and trace topology record. This skill owns evidence gathering and the trace-readiness gate; downstream skills consume the record as read-only input.

## Entry and progressive workflow

1. Find the target `.pdsc`, selected family/subFamily/device/variant scope, affected `<processor>` descriptions, and existing `.agent-artifacts/<pdsc-stem>.debug-topology.md`. Do not create a new DFP and do not continue if no device description exists.
2. Before gathering evidence, read `references/evidence-contract.md`.
3. Load only the target PDSC, selected device subtree, and evidence relevant to the requested connection or trace path. Do not load non-trace sequence records or trace component assets.

## Handoffs

- Hand verified non-sequence PDSC definitions to `$cmsis-pack-debug-description`.
- Hand device-specific non-trace behavior to `$cmsis-pack-debug-sequences`.
- Hand trace assembly to `$cmsis-pack-trace-sequences` only after the record is `READY FOR TRACE`.
- When a change spans these areas, `$cmsis-pack-debug-setup` coordinates the handoffs but starts here.
