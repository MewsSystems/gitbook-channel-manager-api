# Channel Manager side

This section describes the Channel Manager side of the Channel Manager API, i.e. API Operations hosted by the Channel Manager.
The Channel Manager side receives requests from Mews, including inventory updates \(prices, availability and restrictions\) and booking confirmations.

## Environments

Similarly to the Mews side, there should be two environments with different `clientTokens`.
The test/development environment will be used to verify the connection by Mews before connecting to the live/production environment.

## Inventory Update Modes

Mews sends Inventory in two modes, both modes use the same API messages.

### Full Inventory Update Mode

You can request an Inventory update for some specified time period via API operation [Request ARI Update](../mews-operations/operations.md#request-ari-update).
Alternatively, a property employee can use this mode to push the latest data manually.
Data sent in this mode is always for **all** connected rate plans and space types combinations.

### Delta Inventory Update Mode

Mews automatically sends changes in Inventory \(once connection is set up\). Data sent in this mode is just the changed data from the last update.
This is a completely automated process and there is no way to trigger just the delta update to be sent.
Delta inventory updates are sent repeatedly until they are successfully accepted by the channel manager.
