DevGenia 0.14.1 — Readable UI text

Select a text UI instance and use Properties > Text shadow / Text border.
Configure shadow color, transparency and horizontal/vertical offset; configure
border color, transparency and thickness. A thickness of zero disables borders.
Existing projects retain their appearance until an effect is enabled.

Effects follow the letters, including wrapped text, dialogue speakers, progress
percentages, buttons and editable textboxes. Text transparency fades the text
and its effects together; parent opacity is inherited. Background transparency
continues to affect only the background. Studio, Player and the browser renderer
use the same saved properties. See ui-text-effects.txt for scripting examples.

Projects using these properties require project format 11. Older projects still
open normally. New LAN sessions require matching 0.14.1 clients; HTTP clients
negotiate supported protocol versions and reject unsupported project features
instead of dropping them silently. The Player can still join existing hosted
games using the current production server.

This release updates the Windows installation and its signed download feed.
The hosted platform engine is a separate deployment: new-format projects and
browser-side features require that engine upgrade before use on the live site.

Save projects and close Studio/Player before applying an update. Your projects,
edited examples and separate user/account settings are preserved by Setup.
