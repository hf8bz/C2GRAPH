# C2GRAPH v1.4.19 | New feature process guides

> Navigation to public explanatory guides. No audit evidence or test reports are included.

## How these features fit into the older workflow

```text
Existing entry mode / Goal
     |
Current source + bounded discovery
     |
Optional universal capability selection
     |
Requirements, exact write set, approvals and rollback
     |
Authorized implementation (when applicable)
     |
Existing verification + applicable new assurance obligations
     |
Terminal / handoff / suspend, governed by current state
```

**Preserved:** Goal authority, exact write set, rollback, terminal-state truth, and no automatic source modification by public docs.

## Guides

- [Universal Capability Routing](C2GRAPH-UNIVERSAL-CAPABILITY-GUIDE.md)
- [Application Capability Profile and Assurance Completion](C2GRAPH-APPLICATION-ASSURANCE-GUIDE.md)
- [Ledger, Reconciliation and Period-Close Assurance](C2GRAPH-LEDGER-PERIOD-CLOSE-GUIDE.md)
- [Bounded Security Testing and Failure Lineage](C2GRAPH-SECURITY-VERIFICATION-GUIDE.md)
- [Compaction Hooks and Resume Boundaries](C2GRAPH-HOOKS-RESUME-GUIDE.md)
- [Bounded Resource and Temporary Workspace Controls](C2GRAPH-RESOURCE-BOUNDARIES-GUIDE.md)
- [Allowlisted Snapshot, Verification and Restore](C2GRAPH-SNAPSHOT-BACKUP-RESTORE-GUIDE.md)
- [Finite Automated Audit and Release Gate](C2GRAPH-AUDIT-OPERATIONS-GUIDE.md)
- [Guarded Version Migration and Workflow Continuity](C2GRAPH-MIGRATION-UPGRADE-GUIDE.md)

## Where to start

1. Read `README.md` (or `README-ID.md` for C2GRAPH Indonesian) and confirm the active Goal.
2. Choose only the feature guide that matches real project requirements.
3. Follow the canonical runbooks/scripts named in that guide; check all applicable gates.
4. Report unsupported or unverified conditions as such; no universal completion claim.
