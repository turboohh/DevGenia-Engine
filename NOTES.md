DEVGENIA 0.14.7 — CREATOR GAME STORE

Manage Games now offers Entitlements and repeatable Game Sparks. Creators can
upload an icon, set name/description/Genits price, make products public/private
and remove them. Public Game Store cards appear above Game servers. Checkout
reviews the price before an authenticated, idempotent wallet transaction.
The public game page no longer offers Play in browser.

Permanent numeric entitlement IDs are global and never recycled. Deleted and
private products retain paid ownership; scripts can query by numeric ID or key.
Game Sparks have separate repeatable receipts, trusted delivery to the game and
explicit reward acknowledgment. Saved-data reward deduplication handles reconnect
and crash/retry windows; unhandled receipts are retained.

No project-file or public play protocol migration is required. Studio's Java API
includes player.hasEntitlement(long), game.entitlements.has(player,long) and
game.sparks.onPurchase. The installed guides include game-store.txt and
game-store-scripting.txt. Existing Entitlement Simulator testing still works.

Purchases transfer the full Genits price to the game creator. There is no cash
payout or commission. Real-money payment/provider settings are unchanged.
