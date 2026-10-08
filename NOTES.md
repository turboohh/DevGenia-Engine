# DevGenia 0.12.1 - Native GPU by default

Player and Studio playtests now use direct Native GPU presentation automatically
on supported Windows computers. This promotes the existing 0.12.0 renderer after
the application acceptance checks and positive user testing; it does not introduce
a new graphics pipeline or change physics, networking or the simulation timestep.

In **Escape → Settings → Renderer**, the default reads **Automatic (Native GPU)**.
New installations, older preference files without a renderer, and saved Automatic
preferences use it immediately. Existing explicit **Compatibility** and **Native GPU**
choices survive upgrades. The old `NATIVE_EXPERIMENTAL` identifier is retained in
saved preferences for compatibility, with a shorter **Native GPU** display label.

Missing native dependencies, unsupported systems and recoverable renderer errors
retain the compatibility fallback. Unsupported planar mirrors also fall back for
the current viewport. The ordinary Studio editor and its local playtest edit mode
keep their existing picking and transform behavior. Users can still choose
**Compatibility** manually or use the recovery override in `NATIVE_VIEWPORT.md`.

The extended fallback check also exposed a Swing double-buffering error when a
mirror rendered its offscreen reflection during a normal window repaint. The
reflection viewport now paints directly to its existing image without requesting
a window-owned buffer; rendering exceptions on the UI thread fail the native
acceptance test instead of being overlooked.

This update does not reset FPS caps or other client preferences. No additional
FPS gain over selecting Native GPU manually in 0.12.0 is implied.

Only the Windows desktop installer and signed update feed are updated. Platform
services, accounts, games, billing and the release signing identity are preserved.

## Verification and publication

Compiled for Java 17; the final packaged runtime passed 219 renderer-preference,
55 native viewport and 58 actual Studio/Player/Multiplayer host checks. Headless
and missing-native-library preferences were checked separately. The mirror bug
was reproduced by the guarded test on the earlier candidate and is absent from
the final packaged run; 18 visual-environment and 503 editor/face checks passed.

Published installer: 40,885,619 bytes; SHA-256:
`08f222d3715a969839a004aed61203c4c5b0a53f4a012bd33bdf4fe330d8a4d8`.
The actual packaged updater verified the public HTTPS feed, pinned Ed25519
signature and downloaded installer hash.
The platform remained healthy and its JAR hash unchanged.

*Archive copy: internal build and deployment details have been omitted.*
