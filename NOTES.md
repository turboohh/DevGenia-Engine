DEVGENIA 0.14.8 — STUDIO COLORS AND MULTI-SELECTION

The shared Studio color picker now uses a hue/saturation wheel, a brightness
slider and exact HEX/RGB entry. Current and new previews make changes easy to
compare. Apply commits the color; Cancel, Escape and closing the dialog leave
the original intact. Existing alpha values are preserved.

Click inside the arguments of a new Color(...) expression in a script to see a
clickable color preview. The wheel updates that expression's numeric RGB
arguments. Undo and Redo treat the replacement as one change. Comments, strings
and expressions that cannot be safely interpreted as a constant color are not
edited by the preview.

Properties now supports edits across selected Parts and other compatible
instances, including colors, physics switches, numbers, text, layout and instance
references. Each target retains unrelated settings. A shared edit produces one
Undo step, and invalid or stale edits are rejected before changing the selection.
Read-only properties retain their restrictions. Import and editor-launch actions
still operate on the active instance.

Existing projects require no conversion. Studio edits during a playtest remain
temporary and disappear when the test stops.
