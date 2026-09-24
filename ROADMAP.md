# DevGenia development pass

Implementation status for the approved advanced-engine pass. Checked items are working, tested foundations at the scope described below; they are not claims of a production Roblox/Steam replacement.

- [x] Hierarchical transforms, Models and portable independent prefabs; versions 1–3 migration into project format 4.
- [x] Static GLB/glTF geometry import, embedded meshes, software rendering, world-space picking and Point Lights.
- [x] Optional OpenGL renderer implementation, dependency setup and explicit software fallback.
- [ ] Native OpenGL verification on this host: LWJGL/native download did not complete; lib/ is empty. Run the strict check in RENDERING.md after setup.
- [x] Oriented-box physics, mass/friction/restitution, raycasts, triggers, slopes and moving platforms.
- [x] Input actions, events, tweens, WAV audio and bounded X/Z navigation.
- [x] Visual game-UI editor, responsive relative layout and runtime button events.
- [x] Numeric property-keyframe animation editor and playback. Skeletal animation is not implemented.
- [x] Measured script profiler and real JDI debugger with a separate player JVM.
- [x] Autosave/recovery, asset browser and linked-scene transitions.
- [x] Standalone player and portable JAR export with embedded assets and recursive linked levels.
- [x] Local/LAN TCP multiplayer prototype with authoritative simulation and real two-client tests. No public hosting, prediction or remote local-script execution.
- [x] Local platform prototype: password-hashed accounts, owner-controlled immutable publications, pinned library versions and per-account/game progress storage.
- [x] Optional resource-bounded Docker startup-check adapter; tested refusal paths and command/cleanup boundaries. Never falls back to host Java.
- [ ] Live container validation on this host: Docker is not installed. See sandbox/README.md.
- [ ] Interactive isolated execution of untrusted published games. The implemented container check is headless startup validation only.
- [x] Integrated Beacon Run example, full regression suite, documentation and offscreen UI visual verification.

See ENGINE_GUIDE.md for workflows and limits, RENDERING.md for the hybrid GPU path and static importer, and sandbox/README.md for optional OS isolation. Public hosting, matchmaking, marketplace/payment systems, account-linked multiplayer progression, moderation, skeletal animation and a production rendering/physics stack remain further development work.