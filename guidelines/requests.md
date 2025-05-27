# Requests

Both sides of the API accept only `HTTPS POST` requests with `Content-Type` set to `application/json` and with JSON body content depending on the operation to be performed.

## Mews side

All Mews API operations follow this address pattern, with the exception of [Mews: Process group](../mews-operations/reservations.md#process-group):

```
[PlatformAddress]/api/channelManager/v1/[Operation]
```

* **PlatformAddress** - the base address of the Mews platform, which depends on the environment \(e.g. demo, production\)
* **Operation** - the name of the API operation

For each environment, the `clientToken` will be provided to you by Mews. For development purposes, use the [Demo environment](environments.md#demo-environment).

> **Transport Layer Security:**
> For security reasons, Mews API endpoints support TLS security protocol version 1.2 or higher.

## Channel Manager side

All Channel Manager API operations are expected to follow this address pattern:

```
[PlatformAddress]/[Operation]
```

* **PlatformAddress** - the base address of the Channel Manager side of the API
* **Operation** - the name of the API operation

### Providing endpoints to Mews

Please provide Mews with endpoint URLs for the following API operations once they are deployed. Mews needs them for both the Demo environment and the Production environment to be able to send data.
Mews doesn't require all endpoint URLs to be provided at once. We recommend having a different `[PlatformAddress]` for each environment to prevent test data and live data from getting mixed up.
For the same reason, `clientToken` should differ between each environment.

__Required__:

* [Update prices](../channel-manager-operations/inventory.md#update-prices)
* [Update availability](../channel-manager-operations/inventory.md#update-availability)
* [Update restrictions](../channel-manager-operations/inventory.md#update-restrictions)
* [Confirm booking](../channel-manager-operations/reservations.md#confirm-booking)

__Optional__:

* [Change notification](../channel-manager-operations/notifications.md#change-notification)
