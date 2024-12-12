# Inventory updates

This use case explains the two modes by which Mews pushes inventory updates to the Channel Manager side: Full update mode and Delta update mode. Both modes use the same API operations.

## Full Inventory Update Mode

Full Inventory Update Mode sends the complete set of inventory data. In this mode, you request an inventory update for a specified time period using API Operation [Request ARI update](../mews-operations/inventory.md#request-ari-update). Alternatively, a property employee can use this mode to push the latest data manually. Data sent in this mode is always for **all** connected rate plans and space types combinations.

| <div style="width:350px">'How to' use case</div> | API Operations |
| :-- | :-- |
| How to request a full inventory update | [Request ARI update](../mews-operations/inventory.md#request-ari-update) |

## Delta Inventory Update Mode

Delta Inventory Update Mode only sends changes since the last update, optimizing data transfer and ensuring efficient synchronization. In this mode, once the connection is set up, Mews automatically sends deltas or changes in inventory to the Channel Manager. Data sent in this mode is only the data that has changed since the previous update. This is a completely automated process and there is no way to trigger it manually. Delta inventory updates are sent repeatedly until they are successfully accepted by the Channel Manager.
