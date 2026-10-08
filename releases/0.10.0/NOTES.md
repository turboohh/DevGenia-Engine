# DevGenia 0.10 — Core, live Studio and mobile browser play

## Player and Studio presentation

- Heart `damageFeedback` controls a fading red screen vignette. Only the local character's actual health loss triggers it; spawn, healing and other players do not.
- Fixed Windows timer pacing, enabled the bundled GPU renderer by default when available, and reused GPU readback buffers. Software rendering remains the fallback.
- Standalone camera input uses acknowledgement sequences so a stale worker frame cannot undo the latest mouse look. Studio simulation advances at 60 Hz independently of the display FPS setting; detached poses interpolate for presentation.
- Fixed a near-zero normal comparison that could flip flat avatar-face lighting while walking.
- Unlimited, 30, 60, 90, 120, 144, 165 and 240 FPS options. Unlimited removes the display cap; available hardware and scene complexity still determine achieved FPS.
- Refined chat controls and dark channel selector. Removed the persistent bottom-left viewport controls badge and the normal connected Player status strip; disconnect/error messages remain available.
- Menu and leaderboard motion use elapsed time with smooth reversals and respect Reduce motion.

## Building workflows

- Double-click a script identifier or select text to highlight its matching occurrences without replacing the Ctrl+F query.
- Ctrl+G groups eligible selected roots, preserves world transforms, skips services, and supports one-step Undo. Script-only services use a Folder where a Model is invalid.
- Left-drag empty space, or drag with Select, to box-select visible Parts fully inside the rectangle. Ctrl adds to selection. Models are selected as a whole; Alt selects individual Parts; Ctrl+Alt combines both behaviors. The oversized ground beneath a box does not become selected accidentally.
- File → Open shared project → Join live session joins the saved working draft. Authorized developers share scene and script changes, selection/presence and moving camera markers. Publishing remains explicit.
- Nonoverlapping edits merge. A conflicting edit pauses synchronization and preserves the local copy; it does not silently overwrite another developer's code. Stop playtesting or finish a property/transform edit to apply queued shared changes.

## Mobile and server capacity

- Game pages offer Play in browser, with WebGL rendering, touch movement/look/jump, chat, GUI buttons, menu, reset and reconnect. Guests and signed-in users share the existing authoritative server.
- Mobile is an initial browser/PWA client. Native App Store/Google Play packages are not included. Desktop particle/audio/rendering parity and desktop movement prediction are not yet part of the mobile client; see MOBILE_PLAYER.md before marking a game fully mobile compatible.
- Players → Max players sets 1–8 for each saved game revision. The operator can impose a lower ceiling. Normal joins, parties, reservations, queues and server lists use the same frozen room capacity. Existing cloud room limits remain unchanged.

## Verification

The release is checked with isolated Java unit/integration tests, actual Swing gestures and script runtime, native OpenGL rendering, two live Studio clients, browser-cookie authorization, actual mobile-browser touch input, and the packaged Windows launchers. Existing accounts, publications, uploads and server configuration are preserved during deployment.

See CLIENT_PRESENTATION.md, LIVE_STUDIO.md and MOBILE_PLAYER.md for details and current limits.
