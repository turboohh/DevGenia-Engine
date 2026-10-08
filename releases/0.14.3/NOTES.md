DevGenia 0.14.3 — Studio editing, core health HUD and hosted engine compatibility

Select several UI instances of the same type to edit shared appearance properties
in one action. Background transparency, text styling and related appearance fields
apply to the complete selection. Undo restores each element's original values.
Names, text contents and geometry remain individual edits. Mixed-type selections
show the active element's properties without silently editing unrelated types.

CoreService now provides Health HUD enabled, on by default. The built-in HUD shows
the local Character's current/max Heart health and a colored health bar. It reacts
to damage, healing, death and respawn in Studio, Player and mobile web play.
Use game.core.setHealthHudEnabled(false) in a Local Script for a custom player HUD,
or disable the property in Studio for the game. The HUD does not intercept input.

Ctrl+X/C/V cuts, copies and pastes selected instance hierarchies. A pending cut is
shown with faded Explorer rows and can be cancelled with Escape. Cut preserves IDs
and world transforms; copy creates new IDs and remaps internal references across
the selection. Services/runtime-owned player objects cannot be moved this way.
Text fields, script editors and inline renaming retain normal text clipboard use.
Explorer expansion arrows remain visible when a row is hovered or selected.

The previous hosted engine rejected new instances with "Online scene limit:
unsupported instance element". Server compatibility is verified using the expanded
workshop projects through the publication API and OS-confined game workers. Unknown
instance elements are still rejected; errors now identify the element and explain
when the platform engine needs an update. /api/v1/health reports format/protocol.

Modified CoreService settings or references require project format 12. Default-only
CoreService does not force older scenes into a newer project format. Player protocol
10 and snapshot format 12 preserve compatible older Player paths and reject games
that those Players cannot represent, without dropping unsupported semantics.

See ui-multi-selection.txt for shared Properties editing. This increment does not
claim that all of the broader master expansion roadmap has been completed.
