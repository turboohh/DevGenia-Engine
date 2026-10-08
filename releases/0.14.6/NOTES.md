DEVGENIA 0.14.6 — ONLINE LATENCY AND FRAME STABILITY

The server accepts player input while the isolated game worker is calculating
the next world update. Each response uses one complete published snapshot with
matching movement acknowledgements. Input receipts do not claim that commands
have already been simulated. Reconnects, teleports, resets, session expiry and
server shutdown retain their authoritative checks.

When a long stall exceeds the bounded simulation recovery budget, a trusted
server correction cancels stale pending movement. Joining another player also
accounts for time paused by worker setup. Cancelled commands are not reported as
simulated, and the authoritative character position is retained. This prevents
old input from keeping the prediction queue full after the original hitch ends.
Current held movement continues through normal bounded physics during clock
recovery; fresh commands resume the normal timeline. Teleports, death and resets
cancel that temporary recovery mode.

Identical incoming world snapshots reuse the existing decoded scene. New movement
acknowledgements still reconcile prediction even when the scene bytes have not
changed. Changed worlds, damage, chat and correction messages remain validated.

Physics avoids copying complete Parts merely to calculate collision bounds.
Hierarchy queries and replication avoid repeated full-world allocations and
unchanged-field boxing. Server scripts and timed services run once per simulation
slice (up to 1/30 second), independently of a client's display frame rate. Movement
physics, acknowledgements, contact events and server corrections retain their
exact input boundaries. Total simulated time, private views and durable progress
are preserved.

Compact network XML uses bounded direct UTF-8 output, preserving the same bytes,
escaping rules and validation. Incoming snapshots build their project document
during the streaming budget checks, avoiding a second XML parse. Media decoding
still waits for successful validation of the complete document. Already decoded
private views avoid a redundant copy before recipient filtering and encoding.
Hierarchy lookups reuse temporary indexes only
during engine-owned reads; indices are released before user script callbacks or
editor changes. Ordinary saved project files keep their existing formatting.

Routine analytics activity updates no longer wait behind unrelated analytics
file writes. Retention and session-end counters retain their existing semantics.

Project format 12 and play protocol 10 remain unchanged. Existing projects,
accounts and player data do not need conversion. Network travel time and heavy
game scripts can still affect response time; this release removes avoidable
engine waiting and repeated processing rather than bypassing server validation.
