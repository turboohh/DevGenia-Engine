# DevGenia 0.8.3 — playing together and creator workflows

## Player and Studio

- First-person mouse look works without holding a mouse button. Escape, chat, text input and loss of focus release captured input.
- Follow cameras shorten their distance at solid walls and return smoothly when the obstruction clears.
- Stationary Studio views continue animating particle emitters.
- Settings include keyboard remapping, camera inversion, optional standard gamepad input, separate master/music/effects/interface volume, text size and reduced motion. Actual physical-controller behavior still needs hardware testing.
- View → Performance opens a live monitor for frame intervals, render time, simulation time and memory. Existing script profiling remains available.
- Offscreen geometry is culled conservatively. Validated immutable textures, audio and meshes are reused across snapshots using a bounded content cache. Changed content invalidates the cache; mutable scene state remains separate.
- Shared sound files retain separate Sound-instance identities, groups and emitters. Listener and gain changes are not lost when the audio device is busy.
- Online launch shows its real stage and offers cancellation. Temporary connection failures have bounded retries and a manual Reconnect action. Expired or revoked access requires joining again from the website.
- Optional step height, coyote time and jump buffering use the same character controller for prediction and server simulation. Existing projects retain their previous defaults.
- IntValue, StringValue, ObjectValue and LeaderboardStats work in Explorer, Properties, scripts, saved projects and network replication. IntValue children of a Player's LeaderboardStats become Tab leaderboard columns. Player scores are server-controlled.
- The editable obby, time-trial and collecting templates demonstrate Stage, Time ms and Crystals scores.

## Website and accounts

- Sign in can remember a trusted browser for at most 90 days. Logging out, password recovery, access revocation and account restrictions revoke remembered access. Account security provides sign-out-everywhere.
- Profiles have clickable friends/followers/following lists. Account privacy controls protect lists and presence on the server. Online means activity within the last 90 seconds; last-seen information follows the same privacy rules.
- Profile staff badges use the same server-controlled role as game badges.
- Friends can form parties of up to eight, accept invitations and reserve seats in one server. Each member launches with their own identity. Server waiting lists use ordered, expiring reservations.
- Guest game progress is stored separately for each browser guest and can transfer when registering an account. Existing account progress is preserved. Identical save retries are idempotent; conflicting stale saves are rejected.
- Game pages use accessible Play/favorite icons and green Join controls. Studio preview and raw project downloads are restricted to the creator on both the page and API.

## Administration

The server operator explicitly configures an existing owner account. The owner can open `/admin`, search an account and grant or revoke a moderator/admin role, grant Genits, restrict access or end sessions. Sensitive actions require the current password, server authorization and an audit entry. Administrators cannot promote themselves or other accounts.

Account restrictions permit logout and a different account. IP restrictions also affect shared networks. Browser-device restrictions use a signed cookie and can be evaded by changing/clearing the browser; these are **not hardware bans**. This release does not claim invulnerability or add multifactor authentication. See `ADMIN_SECURITY.md`.

## Compatibility and validation

New desktop clients use play protocol 4. The website retains protocols 2 and 3 for previously installed Players. LAN clients must use the same LAN protocol (9). Typed values require project format 6; files without typed values retain format 5. The confined worker protocol is 5 and must ship with the matching server JAR.

Release evidence is recorded in `QUALITY_0_8_3.md` and the build's validation logs. This document describes implementation; production activation is tracked separately there. No universal FPS or latency guarantee is made.
