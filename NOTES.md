DEVGENIA 0.14.4 — INVENTORY AND SERVICE ORGANIZATION

Explorer groups service roots in a stable order: world/player settings, shared and
server assets, scripts and player startup, then UI/input/media and engine tools.
All service icons use the same light-blue color. Ordinary instance colors remain
distinct, and service IDs and project references are unchanged.

During Play, press backtick (`), the key usually above Tab, to open or close your
inventory. It uses the shared Inventory input action, so creators can rebind it.
The default inventory has 24 total slots: nine hotbar slots and 15 extra slots.
Creators can change Inventory Capacity and the Hotbar's NumberOfSlots. Larger
inventories have pages; the maximum remains 120 slots.

With the inventory open, drag an item between a hotbar slot and an inventory slot.
You can also click the source item and then its destination. Occupied destinations
swap their items. Only your own current slots can be changed; stale, repeated or
invalid requests cannot create items. Closing with Escape restores gameplay before
the next Escape opens the player menu. First-person mouse capture releases while
the inventory is open. Chat and other text fields retain normal text input.

The inventory panel uses ordinary DevGenia UI instances, alongside the existing
ordinary-UI hotbar. Server-authoritative inventory contents are separate from the
local open/closed panel, mouse gestures and pagination.

To save the player's inventory and slot order between visits:
1. Insert an Inventory under StarterInventory, or select the existing one.
2. Enable "Save inventory" and choose a stable "Save key" (normally main).
3. Keep Gear/ItemDefinition IDs stable after publishing.
4. For Studio tests, also enable "Keep local test data between Play sessions" in
   View -> Data Inspector before starting Play. Otherwise Stop discards test saves.

The shared DataService saves item IDs, quantities and exact slot positions,
including holes and separate stacks. It stores no character models or transient
gear instance IDs. Saving remains opt-in and unchanged values cause no new writes.
Existing version-1 aggregate saves load and migrate to the version-2 ordered form.
Unknown items, corrupted saves and insufficient save capacity are rejected without
overwriting the existing save or partly changing the inventory.

Operator note: after version-2 inventory saves exist, downgrading the server to
0.14.3 cannot read those ordered saves. Retain a compatible engine for subsequent
rollbacks; never rewind live player data merely to downgrade an executable.
