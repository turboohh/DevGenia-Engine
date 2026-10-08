# DevGenia 0.9 — create, test and improve together

## Start with a playable game

**Help → Build your first game** guides you through the editable **Power Cell Run** example. Change its collectible rewards and vault door, run Play, collect a cell and check the project before publishing. The steps respond to actual editor/game state.

Insert **Door, Collectible, Damage Zone, Quest and Round Manager** components through Explorer. Their Properties configure interaction, rewards, objectives and rounds without rebuilding these systems in every project. Java remains the scripting language. See [Creator kits](CREATOR_KITS.md).

## Keep imported appearance and choose real collision shapes

glTF/GLB imports preserve separate material surfaces, base colors, PNG/JPEG textures, opacity and wrapping. Part Physics now offers Box, Sphere, Capsule and editable Compound colliders, with a wireframe preview. Both rendering backends support imported alpha. See [Materials and colliders](MODEL_IMPORT_AND_COLLIDERS.md).

## Check a draft before releasing it

**File → Publish → Check project** checks scripts, Spawn and component configuration, and project/network size. Errors block release; unfinished work can still be saved as a private draft.

On the website, open **Creator workspace → Manage project** to invite registered testers to one exact revision. They can play and submit categorized feedback. Owners can end a test or remove a tester. Private playtests remain separate from public discovery and saved player progress.

## Share a project safely

Owners grant **Edit**, **Play drafts** and **Publish** independently. **File → Open shared project** opens editable projects in Studio. Each upload makes an immutable revision; a stale upload is rejected and leaves local work intact. History includes authors, and authorized publishers can release or roll back a saved revision. See [Shared projects](SHARED_PROJECTS.md).

## Learn from real play

Game owners get a dashboard with successful joins, failed launches, session length, returning visits, server preparation time and server-interrupted sessions, filtered by version. Server Scripts can mark milestones such as completing a tutorial. Private tests do not inflate these measurements. See [Creator analytics](CREATOR_ANALYTICS.md).

Discover adds **Continue playing**, **Friends are playing**, **Updates to your favorites** and administrator-selected **Featured worlds** when matching content exists. Friend presence and private history retain their privacy rules.

## Compatibility and practical scope

Install the new **DevGeniaSetup.exe** for these Studio/Player features. Existing projects and account data remain supported. Scenes using the new gameplay or collision/material fields use format 7; older Players receive an explicit update message. Game-content updates still load from the platform without reinstalling for each game revision.

This release provides saved-revision collaboration, not simultaneous live co-editing or automatic merging. Model import is static; skeletal animation import and full PBR maps remain future work. Analytics does not claim device FPS, client crashes or end-to-end loading time. Existing server capacity limits remain in force.
