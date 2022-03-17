# API Operations (Mews)

This section describes all operations supported by the Mews side API, organised here by theme.

## Configuration

| <div style="width:200px">Operation or Endpoint</div> | Description |
| :-- | :-- |
| [Get Properties](configuration.md#get-properties) | Get the list of available properties and their connection details |
| [Get Configuration](configuration.md#get-configuration) | Get the configuration of the given property connection |
| [Get Channels](configuration.md#get-channels) | Get the list of all channels with assigned mapping codes |

## Inventory

| <div style="width:200px">Operation or Endpoint</div> | Description |
| :-- | :-- |
| [Set Inventory](inventory.md#set-inventory) | Update the 'rate plan - space type' inventory mapping |
| [Request ARI Update](inventory.md#request-ari-update) | \[`async`\] Request an ARI data update for certain space types and rate plans (ARI is `Availability, Rates and Inventory`) |

## Reservations

| <div style="width:200px">Operation or Endpoint</div> | Description |
| :-- | :-- |
| [Process Group](reservations.md#process-group) | \[`async`\] Process a group of reservations, which can be new bookings, modifications or cancellations. |
