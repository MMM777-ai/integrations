# Bounded pre-commit enforcement example

This example is a thin public composition. It is not OntoGuard's semantic
engine. It shows how an external enforcement runtime can consume a signed
OntoGuard authorization before a protected mutation.

```
destination/tool already admitted by the enforcement runtime
                ↓
exact proposed request
                ↓
verify signed OntoGuard handoff
                ↓
verify exact action-binding digest
                ↓
ALLOW + exact binding → enforcement runtime may continue
BLOCK / ESCALATE / mismatch / expired / tampered /
untrusted / missing authorization → DENY
                ↓
only then may the bounded harness form the protected effect
```

Same permitted capability, different exact actions, already-signed decisions:

| Capability | Exact action | Signed decision | Gate |
|---|---|---|---|
| `RELEASE_PAYMENT` | $250,000 to approved supplier | ALLOW | permit |
| `RELEASE_PAYMENT` | $260,000 to approved supplier | BLOCK | deny |
| `RELEASE_PAYMENT` | $250,000 to newly added supplier | ESCALATE | deny |

Authorizations here are TEST-ONLY harness objects. Tests must pass
`allow_test_keys=True`. The gate default does not accept test keys.

This directory does not contain Headless Runtime, Candidate Standing,
Recovery Standing, semantic packs, or Governing Basis logic.

## What this example proves

An external enforcement runtime can consume a signed OntoGuard decision
before protected execution. The decision is bound to the exact proposed
action. ALLOW for one action does not authorize a materially different
action. BLOCK and ESCALATE do not release. Bypass attempts in this
bounded harness — no authorization, digest-only caller, tampered or
untrusted decision, BLOCK, ESCALATE, or ALLOW for a different action —
do not form the protected effect (`commit_count` stays 0).

## What this example does not prove

It does not prove OntoGuard's internal semantic reasoning, production
network non-bypassability, any third-party gateway integration or
endorsement, hardware attestation, or production L5 guarantees.
Bounded harness non-bypassability is not production route topology.
