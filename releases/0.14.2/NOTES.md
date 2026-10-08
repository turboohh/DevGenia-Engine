DevGenia 0.14.2 — Playtest and interface polish

Proximity prompts are cleared immediately when a Studio playtest ends. Their
fade, hold progress and world anchor are reset before the editor resumes. Moving
out of range during gameplay still uses the normal fade animation. Switching
local scenes also clears the previous scene's transient interaction UI.

The generated Hotbar uses smaller, translucent blue/teal slots and a mint active
indicator. Slot count, top/bottom positioning, keyboard selection, mouse clicks
and ordinary UI customization remain available through the existing systems.

Chat chrome fades away when idle while received messages remain visible. Moving
the pointer over chat, typing or using its controls restores the panel. Message
text retains contrast over the world; the fade does not discard chat history.

NPC look-at uses the actual forward direction and a rigid rotation around the
body root, preserving its position and vertical height. Very close overlapping
targets do not repeatedly flip its heading. This fixes rear-target oscillation
and vertical movement caused by looking from tilted/nested models.

This desktop update includes Studio and Player and preserves existing projects
and user data. The live platform engine remains a separate deployment; existing
hosted games can still be joined, while newer authored systems require the
corresponding hosted-engine upgrade for online publication and execution.
