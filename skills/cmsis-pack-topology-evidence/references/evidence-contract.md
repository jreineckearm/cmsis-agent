# Debug and trace topology evidence contract

Use the current Open-CMSIS-Pack *Debug Description* specification as the grammar authority. This skill collects hardware and topology evidence only; it does not edit PDSC debug-description elements or sequences.

## Topology inventory and evidence

Map the selected device subtree, including inheritance and outer leaf variants. Inspect existing `<debug>`, `<dbg_datapatch>`, `<debugconfig>`, `<debugport>`, `dp_*`, `accessportV1`, `accessportV2`, `<debugvars>`, and non-trace `<sequences>` definitions solely to establish the current topology. Identify every supported trace path, including SWO, synchronous, and trace-buffer paths when documented. Record inherited and local definitions, but do not prescribe or apply PDSC XML here.

Search linked and local documentation first, including datasheets, reference manuals, implementation guides, SVDs, vendor packs, debug scripts, and relevant source code. Record edition/revision and section/page when known; for source code, record repository/version, path, and symbol or line. Do not infer addresses, identifiers, protocol capabilities, core associations, reset/debug-authentication behavior, dormant-state requirements, patches, or trace routing from a part name or a similar device.

If evidence does not establish DP/AP or CoreSight topology, offer `pyocd gdbserver -vv` against an accessible target or request its complete startup log. Automatic detection is positive evidence only: it can omit powered-down, reset-held, debug-locked, or disabled subsystems. Require documented unlock or power-up steps before relying on a repeat scan. For a newer TPIU, confirm whether trace-clock enablement is needed before discovery because accessing an unavailable trace-clock domain can lock the AHB. Treat an incomplete scan as incomplete.

## Required review record

Create or update `.agent-artifacts/<pdsc-stem>.debug-topology.md` at the project root:

```markdown
# CMSIS-Pack debug topology review

PDSC: `<path>`
Device / processor: `<name>`
Selected scope: `<family | subFamily | device | variants>`
Status: `DRAFT — AWAITING USER REVIEW` | `READY` | `BLOCKED`

## Evidence
| Item | Value | Evidence type | Source | Location | Confidence |
|---|---|---|---|---|---|

## Debug connection topology
| Connection model | Processor | DP / AP / APID path | Dormant-state decision | Evidence |
|---|---|---|---|---|

## Trace prerequisites
| CoreSight component instance | Base address | DP / AP path | Evidence | Status |
|---|---|---|---|---|

## Available trace paths
| Path / mode | Processor | Route / components | DP / AP path | Availability | Evidence |
|---|---|---|---|---|---|

## Open questions
- `<question or none>`

## Documents requiring user download
| Title | URL | Retrieval issue | Requested workspace path |
|---|---|---|---|
```

Only this skill may create or update this artifact. Downstream skills use it as read-only input and must return here if a required fact is absent, stale, contradictory, or insufficient.

After collecting evidence, always outline the available trace paths for the user, including unavailable or unresolved paths and their reason. Do this even when trace was not requested. Configure every evidenced path by default after the user confirms the proposed configuration; do not require per-path selection.

## Readiness state

Create and update this agent-owned artifact without confirmation. Before assigning `READY`, present it for user corrections. Set `READY` after confirmation when it contains sufficient evidence for the intended downstream work. Trace assembly additionally requires an evidenced processor, debug connection, DP/AP selection, CoreSight addresses, and every available trace path. Preserve `READY` unless an affected fact becomes stale, contradictory, or insufficient. Before confirmation, use exactly `DRAFT — AWAITING USER REVIEW`; use `BLOCKED` only when a required input is unavailable, and name it.

Do not generate PDSC debug descriptions or trace sequences. Hand verified non-sequence definitions to `$cmsis-pack-debug-description` and trace assembly to `$cmsis-pack-trace-sequences`.
