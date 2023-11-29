# API Operations (CHM)

This section describes all operations supported by the Channel Manager side API, organised here by theme.

## Inventory

| <div style="width:200px">Operation or Endpoint</div> | Description |
| :-- | :-- |
| [Update Prices](inventory.md#update-prices) | This method is used when Mews updates prices of rate plans |
| [Update Availability](inventory.md#update-availability) | This method is used when Mews updates availability of space types |
| [Update Restrictions](inventory.md#update-restrictions) | This method is used when Mews updates restrictions |
| [Process Availability Blocks](availabilityBlock.md) | This method is used when Mews sends availability blocks |

## Reservations

| <div style="width:200px">Operation or Endpoint</div> | Description |
| :-- | :-- |
| [Process Group](reservations.md#process-group) | \[`async`\] Process a group of reservations, which can be new bookings, modifications or cancellations. |
| [Confirm Booking](reservations.md#confirm-booking) | This method is used when Mews confirms a booking sent via [Process Group](../mews-operations/reservations.md#process-group) |

## Notifications

| <div style="width:200px">Operation or Endpoint</div> | Description |
| :-- | :-- |
| [Change Notification](notifications.md#change-notification) | This operation is used by Mews to notify the channel manager when there is a change in the connection configuration |