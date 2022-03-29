# Requests

Both sides of the API accept only `HTTPS POST` requests with Content-Type set to `application/json` and with JSON body content depending on the operation to be performed.

## Mews side

All Mews API Operations follow this address pattern, with the exception of [Process Group](../mews-operations/reservations.md#process-group):

```text
[PlatformAddress]/api/channelManager/v1/[Operation]
```

* **PlatformAddress** - the base address of the MEWS platform, this depends on the environment \(Testing, Staging, Production\)
* **Operation** - the name of the API operation

For each environment, the `clientToken` will be provided to you by Mews. For development purposes, use the [Test Environment](../mews-operations/README.md#test-environment).

### Security

For security reasons, Mews API endpoints support TLS security protocol of version 1.2 or higher.

## Channel Manager side

All Channel Manager API Operations are expected to follow this address pattern:

```text
[PlatformAddress]/[Operation]
```

* **PlatformAddress** - the base address of the channel manager side of the API
* **Operation** - the name of the API operation

> Note: Please provide Mews with endpoint URLs for the following API operations once they are deployed.
> Mews needs them for both Test environment and Production environment to be able to send data.
> Mews doesn't require all endpoint URLs to be provided at once.
> We recommend to have a different `[PlatformAddress]` for each environment to prevent test data and live data getting mixed up;
> for the same reason _`clientToken`_ should differ between each environment.

__Required__:

* [Update Prices](../channel-manager-operations/inventory.md#update-prices)
* [Update Availability](../channel-manager-operations/inventory.md#update-availability)
* [Update Restrictions](../channel-manager-operations/inventory.md#update-restrictions)
* [Confirm Booking](../channel-manager-operations/reservations.md#confirm-booking)

__Optional__:

* [Change Notification](../channel-manager-operations/notifications.md#change-notification)
