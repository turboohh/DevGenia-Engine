# DevGenia 0.8.1 — responsive character movement

The native online and LAN Player predict the local character's walking and jumping on each presentation frame. The camera follows that local pose immediately. Remote players and world objects continue to interpolate authoritative snapshots.

Only bounded input commands are transmitted. The server consumes their duration against its own elapsed simulation time, acknowledges the exact completed and partially consumed commands, and continues to own world physics, interactions, damage, teleportation and respawning. The client replays only unacknowledged input after each snapshot. Small differences are smoothed; lifecycle changes discard obsolete movement. Separate connection epochs and correction counters prevent old commands from being applied after reconnection or reset.

HTTP upload receipts are separate from simulation acknowledgements: received commands leave the upload queue immediately, but remain in the local replay history until actually simulated. A full server queue accepts only a bounded prefix and returns its receipt, allowing the remaining commands to retry without losing the connection or trusting client elapsed time.

Local prediction uses the same movement speed, jump and oriented collision calculations as authoritative character physics, without running game scripts or stepping other players. The snapshot now preserves runtime velocity, including vertical jump/fall speed. Custom rig animations retain server ownership. Walking prediction is suspended for dead, anchored, movement-disabled and seated characters. Vehicle physics remain authoritative. During a stalled connection speculative movement is bounded to approximately 1.5 seconds.

Online protocol 3 adds movement acknowledgements; the server retains protocol 2 support for existing 0.8.0 clients. Those clients need the 0.8.1 Player update to gain local prediction. LAN protocol 8 requires matching host and client versions. Saved .dgs projects are unchanged.


Games without Local Scripts or autoplay audio share the validated world snapshot instead of constructing identical private worlds for each player. Games with private logic or audio retain their dedicated views. Each input upload remains below the server queue budget, independently of the longer local replay history.

## Release verification — 26 September 2026

Version 0.8.1 was deployed to https://devgenia.com. The public installer was downloaded again and verified against the packaged SHA-256. All 562 production classes match between desktop and server JARs.

- Final Windows installer: 116 assertions, including native Studio/Player launch, isolated game execution, upgrades, file associations, uninstall ownership and data preservation.
- Final Linux server: 43 local prediction, 44 movement/collision physics, 48 transport, 51 LAN, 15 simulation timing, 19 input, 64 broker, 30 multiplayer, 25 resource-budget, 31 audio, 61 rig and 77 player-instance checks. Legacy protocol-2 HTTP exchange was also verified.
- Actual confined Linux game worker: 12 movement assertions. Real two-client HTTP tests passed 21 assertions each at 150 ms and 600 ms artificial delay; the 600 ms case included ten seconds of continuous walking. Both finished with zero measured horizontal reconciliation error.
- Website/API/native Player integration under actual Linux confinement: 44 checks.
- Final Windows runtime: 62 avatar-upload and 49 skin/runtime checks with actual isolation. These tests validate effective client scenes whether shared or private.
- Installed Player against the public HTTPS platform: guest launch, movement before another server snapshot, authoritative input acknowledgements, convergence after stopping, reset correction and clean disconnect all passed. Measured stopped horizontal error was zero.

These are functional and synthetic-latency checks, not a guarantee of frame rate or responsiveness for every game or network. Vehicle movement remains authoritative, and a prolonged connection stall still limits speculative movement.

*Archive copy: internal build and deployment details have been omitted.*
