DEVGENIA 0.14.5 — CAMERA, CONTROLS AND ONLINE RESPONSIVENESS

Follow and ThirdPerson cameras now focus toward the character's head, using the
stable body collider rather than animated head motion. The offset scales with the
character and retains creator camera offsets, wall avoidance and first-person
behavior. Studio Play, desktop Player and the mobile browser use the same focus.

All service names and icons in Explorer now use the same light blue, including
CoreService, SharedStorage, ServerStorage and StarterInventory. Ordinary instance
colors remain distinct.

Settings and Report selectors, Studio selectors and dialog buttons use the same
flat dark appearance. Hover, pressed, disabled, selected and keyboard-focus states
remain visible. Keyboard navigation, Enter/Escape and default dialog actions keep
their existing behavior.

Online updates retain server authority for movement validation, damage, inventory
and saves. Local-script presentation is updated at the server's normal simulation
cadence instead of repeating full local-world work for each high-FPS movement
command. Authoritative movement substeps and their acknowledgements remain exact.

The broker validates and sanitizes all incoming worker snapshots, then only builds
the shared outgoing snapshot when a player actually needs it. Private player views
remain separated. Native Player negotiates fast gzip for large HTTP frames; old
clients keep their original response format. Both compressed and expanded bodies
retain the same size limits, and malformed compressed responses fail closed.

Project serialization reuses bounded metadata and streams XML without building an
extra document tree. Read-only physics, replication and local prediction queries
reuse hierarchy indexes within their operation, without retaining stale indexes
across script callbacks or hierarchy changes.

Compatible servers advertise a serial 40 ms polling cadence: processing and
transfer time count toward that interval instead of adding another 40 ms after
every response. Legacy servers retain the previous pacing. Server admission uses
a bounded per-session request budget before simulation-lock waits; authoritative
movement time and action validation remain unchanged.

Project format 12 and play protocol 10 remain unchanged. Existing projects and
player data do not need conversion for this release.
