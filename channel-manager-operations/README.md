# API Operations (CHM)

This section describes the Channel Manager side of the __Mews Channel Manager API__, i.e. API Operations hosted by the Channel Manager. The Channel Manager side receives requests from Mews, including inventory updates \(prices, availability and restrictions\) and booking confirmations. The list of supported operations is as follows, organised here by theme.
 
> ### Inventory update modes
> Mews pushes inventory to the Channel Manager side in one of two modes, both using the same API Operations, either **Full Inventory Update Mode** or **Delta Inventory Update Mode**.
> For more information, see [Use cases > Inventory updates](../use-cases/inventory-updates.md).

## Inventory

| <div style="width:200px">Operation</div> | Description |
| :-- | :-- |
| [Update prices](inventory.md#update-prices) | This method is used when Mews updates prices of rate plans |
| [Update availability](inventory.md#update-availability) | This method is used when Mews updates availability of space types |
| [Update restrictions](inventory.md#update-restrictions) | This method is used when Mews updates restrictions |
| [Process availability block](availabilityBlock.md) | This method is used when Mews sends availability blocks |

## Reservations

| <div style="width:200px">Operation</div> | Description |
| :-- | :-- |
| [Process group](reservations.md#process-group) | \[`async`\] Process a group of reservations, which can be new bookings, modifications or cancellations. |
| [Confirm booking](reservations.md#confirm-booking) | This method is used when Mews confirms a booking sent via [Process group](../mews-operations/reservations.md#process-group) |

## Notifications

| <div style="width:200px">Operation</div> | Description |
| :-- | :-- |
| [Change notification](notifications.md#change-notification) | This operation is used by Mews to notify the channel manager when there is a change in the connection configuration |
