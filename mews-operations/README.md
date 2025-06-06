# API Operations (Mews)

This section describes the Mews side of the __Mews Channel Manager API__, i.e. API Operations hosted by Mews. The Mews side receives requests from external Channel Managers, including requests to process reservations (new, updated and cancelled). The list of supported operations is as follows, organised here by theme.

## Configuration

| <div style="width:200px">Operation</div> | Description |
| :-- | :-- |
| [Get properties](configuration.md#get-properties) | Get the list of available properties and their connection details |
| [Get configuration](configuration.md#get-configuration) | Get the configuration of the given property connection |
| [Get channels](configuration.md#get-channels) | Get the list of all channels with assigned mapping codes |

## Inventory

| <div style="width:200px">Operation</div> | Description |
| :-- | :-- |
| [Set inventory](inventory.md#set-inventory) | Update the 'rate plan - space type' inventory mapping |
| [Request ARI update](inventory.md#request-ari-update) | \[`async`\] Request an ARI data update for certain space types and rate plans (ARI is `Availability, Rates and Inventory`) |
| [Confirm availability update](inventory.md#confirm-availability-update) | Confirms availability update was processed succesfully or with errors. |
| [Confirm price update](inventory.md#confirm-price-update) | Confirms price update was processed succesfully or with errors. |
| [Confirm restriction update](inventory.md##confirm-reistriction-update) | Confirms restriction update was processed succesfully or with errors. |
| [Confirm availability block synchronization](availabilityBlock.md##confirm-availability-block-confirmation) | Confirms availability block synchronization was processed succesfully or with errors. |
| [Process availability block](availabilityBlock.md#process-availability-block) | Add, update or cancel an availability block. |

## Reservations

| <div style="width:200px">Operation</div> | Description |
| :-- | :-- |
| [Process group](reservations.md#process-group) | \[`async`\] Process a group of reservations, which can be new bookings, modifications or cancellations. |
| [Confirm reservations group synchronization](reservations.md#confirm-group-confirmation) | Confirms reservations group synchronization was processed succesfully or with errors. |


