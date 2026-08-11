---
name: cmsis-pack-trace-sequences
description: Add modular, evidence-backed CoreSight trace sequences to an existing CMSIS Device Family Pack PDSC. Use after `$cmsis-pack-topology-evidence` has produced a READY topology record that evidences available trace paths, when assembling per-component trace snippets into TraceStart, TraceCapture, TraceFlush, and TraceStop sequences, or when maintaining trace setup with device-specific extension placeholders.
---

# CMSIS-Pack trace sequences

Assemble modular CoreSight trace snippets into an existing PDSC. Read the matching `.agent-artifacts/<pdsc-stem>.debug-topology.md`, then read `references/component-index.md` and only the selected component assets at the skill-root-relative paths listed there (for example, `assets/components/etf.xml`). Use `assets/scaffolds/full-from-scratch.xml` for new full-mode sequences; read `references/legacy-to-full-migration.md` and use `assets/scaffolds/legacy-to-full.xml` when converting legacy mode.

## Preconditions

1. Find the target PDSC, selected family/subFamily/device/variant scope, and matching read-only `.agent-artifacts/<pdsc-stem>.debug-topology.md` review record from `$cmsis-pack-topology-evidence`. Require `Status: READY` plus evidence for every available trace path. Cross-check the component instances, addresses, DP/AP paths, recorded placement scope, and explicit `debugconfig`/`debug` elements against the PDSC. If the record is absent, blocked, stale, contradictory, or insufficient for an available path, stop and direct the user to `$cmsis-pack-topology-evidence`.
2. Generate and maintain the target `<sequences traceSetup="full">` configuration. Do not generate `legacy` trace setup; if the existing PDSC declares `legacy`, report that it will be converted to `full` as part of the trace change.
3. Treat trace setup as device-level by default, not CPU-level. Unless the user explicitly narrows the scope, consider all processors in the selected device subtree and every evidence-backed trace path the device supports, including SWO, synchronous trace, and trace-buffer variants.

## Progressive assembly

1. After the preconditions establish the selected paths, read `references/component-index.md` and load only the component assets required by those paths.
2. Before generating or editing PDSC XML, read `references/assembly-contract.md`. It owns component selection and placement, routing, scaffold structure, XML formatting, evidence limits, asset fidelity, confirmation, and validation. Follow it completely; do not restate or load unrelated component assets.
3. Use the selected scaffold and component assets only after reading the assembly contract. Load `references/legacy-to-full-migration.md` only when converting a legacy configuration.
