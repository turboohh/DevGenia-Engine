# DevGenia 0.11.1 — Large-window rendering and fullscreen

Player and Studio playtests now combine the sky, celestial bodies and transparent world geometry on the GPU before returning an opaque RGB image to Swing. The previous path copied a transparent frame back to the CPU and scaled/blended it over a full-window background. That extra alpha-compositing work grew with window size. Native output is still from the current camera frame, with no previous-frame readback or extra input delay. Resolution budgets, quality settings, simulation, prediction and the FPS cap are unchanged. HUD text remains at native window resolution. Celestial edge antialiasing can differ slightly from the old Java2D circles.

Editor rendering retains its transparent surface and pick buffer. Mirrors/software rendering retain their existing background/compositing path. A failed native frame now preserves the complete presentation cache while rendering its software fallback, rather than discarding the cache mid-paint.

F11 toggles borderless fullscreen in standalone Player, online/LAN Player and Studio. The same component tree, runtime, connection and GPU context remain active. Returning restores normal/maximized bounds and focus. Held keys are released during the transition, repeated F11 presses from key-repeat are ignored, and Escape continues to open the game menu.

## Validation

- 188 native background checks: day/night, logical-pixel sun/moon projection, glass and texture alpha, depth, particles, editor/play transitions, resize, clearing, native failure and software recovery.
- 118 native fullscreen checks: active render clock and runtime ticks, first-person capture, visible Escape menu, preserved OpenGL context, F11 repeat protection, window bounds, and close/disconnect lifecycle. Physical monitor removal was covered by geometry fixtures, not every hardware/DPI configuration.
- Existing renderer, quality/picking, alpha, presentation and workflow suites passed, including 62 real-window FPS-cap checks. The code was compiled for Java 17.
- Complete Player offscreen paint on this GTX 1650: 720p median 13.88→12.16 ms; 1080p 22.99→14.44 ms. These are render timings, not measured on-screen FPS.
- In a quiet, visible rotating-camera test at Medium/Unlimited, a 640×360 viewport measured 124.9→130.8 FPS and a maximized 1920×971 viewport 41.6→44.0 FPS. An earlier 27.8→42.7 FPS result coincided with compilation and is not used as a reliable improvement claim. GPU readback and window presentation still constrain large-window throughput; this release does not guarantee 60 FPS on every configuration.
- Disabling the Java2D D3D path reduced maximized performance to 35.7 FPS. Temporary whole-interface RGB composition and direct raw-frame presentation also gave no improvement; these experiments were not shipped. JFR identified remaining readback, upload and presentation costs.
- The isolated Windows installer passed 116 install/launch/upgrade/repair/uninstall checks, including preserved user projects and unchanged production file/URL associations. A packaged client downloaded and verified the public HTTPS release with its pinned Ed25519 key and SHA-256. The normal website download was independently checked against the same installer.

The desktop-only release replaces the public installer and signed update metadata without restarting the platform or migrating account/game data. Install the desktop update to receive these changes; an already-running older executable cannot load them.

Published installer: 40,788,578 bytes; SHA-256 `560a1bac3271d6f6383f856aff9309feadf1e9d73519fe93b585cff0a3adf250`. The live server JAR was not replaced.

*Archive copy: internal build and deployment details have been omitted.*
