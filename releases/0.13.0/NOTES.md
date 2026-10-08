# DevGenia 0.13.0 — launch quality

This release improves shared game simulation, creator onboarding, analytics, moderation capacity and mobile presentation. Native GPU remains the default Windows play renderer, with the existing compatibility fallback. It does not activate payments or certify readiness for a broad public launch.

## Player and creator changes

- Identical public world snapshots are encoded once per worker step and shared with clients. Local camera, UI, audio, physics and other script changes still receive their own view. Server scripts remain private. Authoritative replication compares each server delta once and applies it separately to each local world.
- Short-lived hierarchy lookup scopes reduce repeated scene scans during serialization and rig queries. They end before editor changes or script callbacks; no stale cross-frame scene cache is retained.
- Power Cell Run is a complete reference loop: collect three distinct cells together, power the gate, extract, then replay. It includes a readable HUD, embedded sound cues, durable wins/best time and a creator playtest checklist. Exported examples are not automatically published as public games.
- First-game checks are invalidated when the project changes, so an old successful preflight cannot approve a different revision.
- Mobile browser play gains local text-field editing, embedded UI imagery, bounded particles/audio, mute/resume and touch cancellation. This is browser support; native iOS/Android packages and physical-device certification are not included.

## Platform and operations

- Creator analytics use first-observed UTC cohorts for D1/D7 retention, with mature denominators and explicit incomplete-history limits. Old unidentified activity is not presented as new acquisition.
- A bounded, authenticated first-render acknowledgement measures the time from launch-ticket issuance to the server receiving a client's first visible rendered-frame report. It is client-reported render evidence, not proof of responsive input or display scanout. Private previews are excluded.
- Expired restrictions no longer consume the active restriction limit forever. Original evidence is preserved in private history. Corrupt restrictions fail closed.
- Reviewed reports leave the pending report capacity while immutable originals remain available for audit and per-reporter quotas. Rebuildable indexes recover interrupted index maintenance. This is not a transaction journal for the entire enforcement/audit workflow, and archive scans still grow with history.
- Resend operational alerts support bounded delivery, stable retry identities, cooldowns and recovery notifications. On-server monitoring cannot report a complete outage of its own VPS; external uptime monitoring is still needed.
- A read-only billing preflight reports ledger capacity, receipt attention and reconciliation work without contacting Stripe or changing balances. Checkout reconciliation handles an already-paid session even if its webhook was missed, using existing fulfillment deduplication. Payment and membership sales remain disabled pending owner/provider approval.
- Server deployment has checksum checks, a private deployment lock, a consistent backup and verified artifact rollback. It does not freeze new admissions before the service stops, and artifact rollback does not undo data/schema changes.

## Validation and remaining limits

Tests include actual Windows and Linux isolated workers, HTTP multiplayer/protocol tests, native desktop/browser presentation checks, mocked payment lifecycle scenarios and deployment fault injection.

The CX23 eight-client worker workload improved substantially but still misses the 30-step/s target. Worker throughput is separate from GPU FPS and is not a promise about real-player latency. Keep capacity conservative and run an outside multi-device beta before scaling.

Legal/operator details, age and verified parental-consent handling for under-13 access, external security review, Authenticode, physical mobile testing and live Stripe verification remain explicit gates. Every staff member must complete MFA. No test result substitutes for these approvals or real user acceptance.

*Archive copy: internal build and deployment details have been omitted.*
