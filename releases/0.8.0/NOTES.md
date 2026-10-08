# DevGenia 0.8.0

Published to https://devgenia.com. Existing accounts, games and uploads are preserved.

## Player and Studio

Server simulation now advances with monotonic elapsed time and bounded substeps instead of sleeping another fixed frame after each update. Online input polling is faster. Declarative snapshots use bounded in-memory XML rather than temporary disk files, retaining the existing XML and scene validation. A 46-part fixture improved median serialization/deserialization from 17.07 to 8.84 ms; this is not a gameplay FPS benchmark. A real confined Linux worker travelled at 98.3% of configured movement speed over 2.505 seconds, including snapshot overhead.

Escape offers Reset avatar for online, local and Studio sessions. Published online servers offer an Invite friends link with a public branded Open Graph preview; it contains no play credentials and expires when the room/current release is unavailable. Server lists show occupant avatars. Attachment-based spatial audio survives the isolated worker's client filtering.

The installer associates unowned .dgs files with Studio, registers Open With and the Studio icon, preserves existing Windows choices, and cleans up only its own registrations.

## Website

Catalog items have durable numeric IDs per category, e.g. /catalog/shirts/1 and /catalog/faces/1. Legacy references, saved appearances and protected templates keep working. Profiles support following and visible group memberships/ranks. Friends and Messages have numbered red badges; new messages can trigger a DevGenia notification sound after browser interaction. Conversations can be muted; Enter sends and Shift+Enter inserts a newline.

Groups support photos, Owner/Admin/Member defaults, configurable ranks and permissions. Additional ranks cost 10 Genits with idempotent charging and recovery. The website serves SVG and PNG favicons.

## Payments

Stripe Checkout is prepared for Genits with signed callbacks, verified provider receipts, idempotency and refund/dispute handling. Payments remain disabled: no live credentials or real charge has been used. An activated merchant account and private setup are still required. DevGenia+ subscription billing is not enabled. See COMMERCE.md and deployment/configure-stripe.py.

## Validation

84 Java regression suites and 10 JavaScript behavioral checks passed. Final focused billing tests: 183 checks; simulation/snapshot tests: 15. Final Linux Java 17/rootless Docker tests passed, including the native HTTP player flow. The actual Windows installer passed 116 assertions, including native Studio/Player launch, AppContainer, upgrade/uninstall, file association and data preservation. All 554 production classes match between the installed desktop application and the deployed server. Public HTTPS download hash and native guest launch/reset/invite were verified after activation.

*Archive copy: internal build and deployment details have been omitted.*
