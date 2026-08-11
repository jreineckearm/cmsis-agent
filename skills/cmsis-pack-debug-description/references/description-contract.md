# Non-sequence debug-description contract

Use the current Open-CMSIS-Pack *Debug Description* specification as the grammar authority. Use `.agent-artifacts/<pdsc-stem>.debug-topology.md` from `$cmsis-pack-topology-evidence` as read-only input; never create, update, or feed PDSC implementation details back into that record.

## PDSC definitions

Translate only verified topology facts into inherited/local non-sequence PDSC definitions. Provide explicit `debugconfig` and `debug` elements for every verified connection model. Omit optional debug-port and access-port definitions that only restate the standard default model; add `accessportV1` only when the verified topology requires it, including a mixed ADIv5/ADIv6 design. Apply `debugconfig dormant="true"` only when the topology record contains device-specific evidence.

Apply verified `dbg_datapatch` entries, including their location, value or payload, applicability, and workaround. Preserve trace-owned `debugvars` and all sequences. Do not use `__` as a prefix for DFP-defined local sequence variables.

Place a shared definition at the highest selected common device-tree ancestor; place a deviation only on its outer leaf variant. Preserve unrelated PDSC content.

## Confirmation and validation

Before editing a user-owned PDSC, present the referenced topology record and proposed XML for confirmation. Apply only confirmed facts. Validate XML/PDSC syntax with the available toolchain, then run:

```text
rg -n '<block>.*;</block>' <target.pdsc>
rg -n '^[[:space:]]*[^<].*;.*;' <target.pdsc>
```

Report emitted definitions, their placement and applicability, sources, confidence, unresolved items, and validation results. If the requested edit exposes a missing or contradictory hardware fact, return to `$cmsis-pack-topology-evidence`; do not repair the topology record here.
