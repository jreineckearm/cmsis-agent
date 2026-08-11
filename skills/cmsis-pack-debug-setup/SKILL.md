---
name: cmsis-pack-debug-setup
description: Coordinate complete evidence-backed CMSIS-Pack debug setup in an existing Device Family Pack PDSC by running topology-evidence, debug-description, optional debug-sequence, and optional trace-sequence skills in order. Use when a change spans two or more of these areas and requires user confirmation after each specialist step.
---

# CMSIS-Pack debug setup

Run the specialist skills internally as one user-facing workflow. Each specialist retains ownership of its own rules, records, XML, and validation.

## Entry

1. Find the target PDSC and selected family/subFamily/device/variant scope. Confirm that the PDSC already contains device descriptions; do not create a new DFP.
2. Retain the user's selected scope. Configure every evidenced non-sequence definition, device-specific behavior, and available trace path by default; Topology Evidence identifies the trace paths.

## Ordered specialist calls

1. Call `$cmsis-pack-topology-evidence`. It creates or refreshes the read-only `.agent-artifacts/<pdsc-stem>.debug-topology.md` record. Present the completed record and its available trace paths, then obtain the user's confirmation or corrections before continuing. Do not call a downstream specialist while the record is `DRAFT` or `BLOCKED`, or lacks facts required for that specialist.
2. Call `$cmsis-pack-debug-description` for verified non-sequence PDSC definitions. It consumes the topology record read-only. Present its proposed XML and result, obtain the user's confirmation, then apply and validate its change before continuing. If no definition is required, explicitly report that result and obtain confirmation to continue.
3. Call `$cmsis-pack-debug-sequences` for every verified device-specific non-trace behavior, including reliable debug through supported low-power modes. Present its proposed XML and result, obtain the user's confirmation, then apply and validate its change before continuing.
4. Call `$cmsis-pack-trace-sequences` after the topology record is `READY` for every evidenced available trace path. It reads the same topology record directly. Present its proposed XML and result, obtain the user's confirmation, then apply and validate its change.

## Coordination rules

- Keep the data flow one-way: only Topology Evidence updates `debug-topology.md`; every other specialist reads it.
- Configure every evidenced capability by default. When a documented runtime choice belongs to the end user, expose it through `debugvars` with a supported default instead of asking the user to select implementation paths.
- Do not combine confirmations. Wait for the user's response after each completed specialist call before starting the next one.
- If a specialist lacks evidence, revisit the appropriate preceding specialist within this workflow. Ask the user only for the missing technical input.
- Do not duplicate specialist instructions, load trace assets before calling Trace Sequences, or move ownership of PDSC content between specialists.
