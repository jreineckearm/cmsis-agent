---
name: cmsis-pack-debug-description
description: Add or review non-sequence CMSIS-Pack Debug Description content in an existing Device Family Pack PDSC. Use for debug, debugconfig, dbg_datapatch, non-trace debugvars, debug-port, or access-port definitions after `$cmsis-pack-topology-evidence` has established the required topology. Do not collect topology evidence, implement device debug sequences, or generate CoreSight trace sequences.
---

# CMSIS-Pack debug description

Apply verified non-sequence debug-description definitions to an existing DFP. `$cmsis-pack-topology-evidence` owns the upstream topology record; device-specific non-trace sequences belong to `$cmsis-pack-debug-sequences`; trace sequence generation belongs to `$cmsis-pack-trace-sequences`.

## Entry and progressive workflow

1. Find the target `.pdsc`, selected family/subFamily/device/variant scope, affected `<processor>` descriptions, and `.agent-artifacts/<pdsc-stem>.debug-topology.md` produced by `$cmsis-pack-topology-evidence`. Do not create a new DFP and do not continue if no device description exists.
2. Require a current topology record covering the selected connection and requested trace prerequisites. If it is absent, blocked, stale, contradictory, or insufficient, return to `$cmsis-pack-topology-evidence`.
3. Before editing, read `references/description-contract.md`. Load only the target PDSC, selected device subtree, and relevant topology-record rows. Do not load non-trace sequence records or trace component assets in this skill.
