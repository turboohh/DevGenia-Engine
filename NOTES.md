# DevGenia 0.12.0 — Direct GPU presentation

The Windows Player and Studio playtest can now present the 3D world directly through
an OpenGL Canvas. World frames stay on the GPU: there is no `glReadPixels`, CPU row
flip or full-world Swing upload in the new presentation path. The current geometry,
materials, lighting, quality budgets, simulation and movement prediction are retained.

The actual Swing chat, game UI and Escape menu remain in the same window and keep
their existing state and input handling. Their transparent appearance is captured
once per requested frame and composited on the GPU. Text input retains its live
caret; mouse, scrolling, drag capture, tooltips and context menus reach the original
controls. This does not introduce a queued or delayed world frame.

Native context creation, rendering and cleanup stay on the event thread in this
release. That keeps the current mutable scene and UI ownership intact. F11 recreates
only the native drawing context when the Canvas peer changes; the game session,
connection, editor document and controls continue. Hidden or minimized native
surfaces do not generate rendered-frame counts. The existing presentation clock
still controls frame limits, and FPS measurements count completed native swaps.

Settings now include a persisted renderer choice and a compatibility recovery option.
For this first rollout, **Automatic keeps the established renderer**. Enable the new
path explicitly in **Escape → Settings → Renderer → Native GPU (experimental)**;
choose **Compatibility** to switch back without restarting the game. Native results
have been verified on the available Windows/NVIDIA machine, not across all vendors.
The existing editor renderer remains responsible for authoring and local play-edit
tools. Unsupported native features and recoverable initialization/draw failures
restore the established renderer. See [NATIVE_VIEWPORT.md](NATIVE_VIEWPORT.md) for
dependencies, developer commands and recovery instructions.

## Validation

- Java 17 compilation remains possible without compile-time LWJGL dependencies in
  core. The optional native bridge is built separately against each package's fresh
  core, with pinned LWJGL/JAWT dependencies and license notices.
- Existing rendering tests passed: 188 GPU background, 16 compositing, 21 quality,
  15 transition, 39 presentation, 20 texture-alpha and 9 imported-material checks.
- Native checks include renderer preference persistence, real Swing HUD input and
  caret, native context lifecycle, fullscreen reparenting and compatibility fallback.
- 199 preference checks, 31 retained-HUD input checks, and 42 native surface checks
  passed. Another 55 checks cover the actual Studio, Player and MultiplayerWindow
  hosts. Surface and host suites also passed at forced 150% Java DPI scaling, with
  screenshots confirming world pixels, text and premultiplied HUD transparency.
  The multiplayer host used an isolated local GameClient fixture; this is not a new
  production network load test. Mixed-DPI monitor hot-plug and other GPU vendors
  have not been separately verified.
- Final visible Player comparison on a GTX 1650, Medium/Unlimited, rotating
  camera and the same starter scene: 640×360 measured 145.6 → 167.9 FPS; maximized
  1920×971 measured 45.3 → 74.9 FPS; F11's 1920×1034 viewport measured 40.3 → 83.0
  FPS. Each measured sample lasted four seconds after warmup, without concurrent
  compilation or GPU tests. These are local measurements, not promises for every
  GPU, game, resolution or driver. Earlier runs and full raw output are retained.

The full-window transparent-JWindow prototype was rejected after its HUD updates
introduced substantial presentation overhead. The shipped approach retains the HUD
hierarchy in the original window and composites the captured HUD on the GPU.


## Published release

The isolated Windows installer passed **121 assertions**, including installed Studio
and Player startup, native dependencies resolved through the actual packaged
manifest, repair/uninstall protection and preservation of production associations.
The packaged runtime then passed the 42 native surface checks with its application
JAR first on the classpath and without developer `lib/*` dependencies.

Additional regression suites passed: 62 frame-limit, 58 viewport workflow, 38 menu,
149 TextBox runtime, 48 chat-bubble/privacy, 43 movement-prediction, four controller
and 119 compatibility-fullscreen checks. The legacy fullscreen fixture now waits
for real keyboard focus before synthesizing F11; its previous immediate key
injection could race Windows activation. No production fullscreen behavior was
changed for that fixture fix.

Version **0.12.0** is published through the website download and pinned Ed25519 update
feed. Installer size: **40,888,717 bytes**. SHA-256:
`17944dd8c66a071b45eee8abfbcf36d0321d17c33b2a3daeceade39594946a20`.
The actual packaged updater verified and downloaded the public release without
executing it; the normal website download was independently checked over validated
HTTPS against the same hash. The existing release signing identity was preserved.

Only desktop release files were deployed. The platform stayed healthy and its JAR
hash remained unchanged; no account/game migration or service restart occurred.

*Archive copy: internal build and deployment details have been omitted.*
