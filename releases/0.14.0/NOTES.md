DevGenia 0.14.0 — Master Expansion development release

This installer updates the ordinary Windows Studio and Player installation.
It includes Java, the matching native GPU adapter, sandbox helper, three new
editable examples and the expansion guides. Studio displays 0.14.0 at the
bottom right, making an old shortcut or old installer easier to recognize.

File > New from template:
  Gear Workshop: equipment, welds, inventory, hotbar and interaction.
  Quest Expedition: NPC dialogue, quests, rewards and a small supply shop.
  Race Workshop: two laps, ordered checkpoints, HUD and saved best times.

The shared engine APIs, expanded UI authoring, data tools, simulator, seats,
vehicles, camera timelines, lights and local scene transitions are described
in the accompanying guides. The 90-point Master Expansion is still in progress;
master-expansion-status.txt distinguishes implemented systems and open work.

The desktop Player can join the existing protocol5 platform as well as the
new protocol8 server. Negotiation occurs only on the exact pre-ticket protocol
refusal; authentication failures never trigger retries or weaker authorization.
Legacy movement commands and local prediction keep their original movement
semantics, while script keys still use the old extra-key channel.

This desktop release does not upgrade the hosted game engine. Publishing and
running new format-8/9/10 engine features online requires a corresponding
platform engine upgrade. These new systems can be used now in Studio playtests,
local standalone games and current-version local test servers.

Setup preserves projects, edited bundled examples and separate account/settings
data. Save open projects and close Studio/Player before applying an update.
Old downloaded installers still contain their old engine; use the 0.14.0 setup.
